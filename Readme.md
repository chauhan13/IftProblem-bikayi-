## starting

to request a lift when peoples press a button 

```
req={
	floorNo
	direction
	liftassign:liftId
	
}


lift={
	liftId
	state : "idle/ up/ down"
	Capacity : true / false
	floorNo
	stops : [ ] 
	Stopsassign: odd or even including UB,LB,1,Top floor
}
```



## Algorithm 

assigning lift in floor ex- if NO 0f floors is 200 and  lifts=20
then lift per floor = no of lift /2 ;
			first 10 lift at even floor including 1, UB,LB and Top Floor.
			last 10 lift at odd floor including 1,UB,LB and Top Floor
			

And there are only two buttons in every floor one is to access the odd lifts and one is to access even lifts.
			
Initialize all lift ->

```
{
	liftId= 1, 2, etc.  
	state = idle
	fullCapacity = false
	floorNo = 0, 1, etc.
	stops [ ]  - emptyarray
}

```
			


When a request comes, check for the nearest lift and assign the request to the lift. 
The nearest lift is chosen by checking lifts 
-  without full capacity
-  difference of floors with current passenger floor
-  if an lift is idle or the lift going in the same direction as the passenger. 
-  reassign an already assigned lift if lift is fully loaded.



```
for each request in req array {
	assignlift(lifts, Request)
}

```
Assign an lift to every person


```
assignlift(lifts, Request) {
	nearestlift = findNearestlift(lifts, Request) 
	nearestlift.stops.add(request.floorNo)
	if (lift.state == 'idle')
		lift.state = request.direction
	Request.assigned = liftId
}

```


function to find the nearest lift based on lift's current  position, direction, and capacity

```
findNearestlift(lift, Request) {
	nearestlift = null
	minFloorDiff = INTMAX32
	for each lift (
		if (fullCapacity == true)
			continue;
		if (request.direction == lift.state  || lift.state == 'IDLE') {
			if (lift.state = up and lift.floorNo < request.floorNo)
				floorDiff = request.floorNo - lift.floorNo
			else if (lift.state = down and request.floorNo < lift.floorNo)
				floorDiff =   lift.floorNo - request.floorNo
			else 
				wait till an lift is idle or an lift changes direction
			if (floorDiff < minFloorDiff) {
				minFloorDiff = floorDiff
				nearestlift = lift
			}
		} 
	}
	return nearestlift;
}
```

Method called when lift stops at a floor. We need to reassign a person request if lift becomes full.

```
updateliftState(lift) {
	remove floorNo from stops array;
	if (stops array is empty)
		lift.state = idle
	check lift capacity is not full
		lift.Capacity = false
	else
		lift.fullCapacity = true
	if lift.Capacity == true  {
		for each request with liftId == lift.liftId {
			//reassign lift
			assignlift(lifts, request)
		}
	}
}
```


function called when a person presses a button after entering the lift 

```


addFloorTolift(Elevator, floorNo) {
	lift.stops.add(floorNo)
	check lift capacity is full
		if full
			lift.Capacity = true
	remove request from request array by searching requests array with the assigned lift No
}

```

For peek time. morning 8am - 9am, evening 6pm - 9pm run below code

```
for each elevators {	
	if (lift.idle) {
		if  time between 8 to 9
			lift.state = down until floorNo is UB
		if time between 6 to 9
			lift.state = up until floorNo is top floor (200)
	}
}
```

