## angular-activity-monitor
========================

An activity monitoring service and activity monitor directive. It's cool!


### Components:
1) A monitoring service that plugs into your controller
2) A directive that subscribes to a monitoring service ('namespaced')


### Monitoring Service:
==================

#### Service Provider (injected into controller or directive)
##### Methods:
* createServiceInstance('service.instance.name')
  - returns the service instance
* getServiceInstance('service.instance.name')
  - returns undefined if no match is found
  - returns the service instance if it exists
* getServiceInstancesStartingWith('service')
  - returns array of all service instances starting with 'service'
  - returns an empty array if no matches found

#### Service Object

#####private attributes
* propertiesToWatch
  - the properties that we wish to watch
* lastSynchronizedObj
  - the latest thing we know about the object from the server
* watchDestroyers
  - array of functions to kill the $watch-es on propertiesToWatch
* updateInProgress
  - an update is currently being sent and has not returned yet
* updateQueued
  - an update is currently being sent AND the next update is already waiting to be sent


##### Methods:
* init(arrayOfPropertiesToWatch, referenceToScopeObjToWatch, initialObjValues)
  - ['hourly_wage', 'training_hourly_wage']
  - $scope.usersPosition
  - {hourly_wage: 0, training_hourly_wage: 0}
* monitoringOn()
  - turn on the watches on the properties above
* monitoringOff()
  - turn off the watches on the properties above
* isMonitoring()
  - are the watches on or off?
  - _.any(watchDestroyers, _.isUndefined);
* updateSynchronizedState(obj)
  - update the lastSynchronized object
  - lastSynchronizedObj = obj;
* queueUpdate(theUpdateFunctionToCall)
  - returns a GUID for that request
  - checks whether there is an update already going on. If so, watches updateInProgress. When watch goes off and updateInProgress === false then call theUpdateFunctionToCall
  - if updateInProgress === false then just call theUpdateFunctionToCall
* requestCompleted(theRequestsGuid, dataFromUpdate)
