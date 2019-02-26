## Please use code, psuedo-code, and/or text to answer the following questions. You will be evaluated based on the quality of your response. There is not necessarily a wrong answer/approach.


1. You have just received a job in charge of the flight planning system for a drone delivery service. You're responsible for designing a system that when given a virtual map of an area can map out a flight path for the drone that will maximize the number of customers served. 

*Problem:*

Write a function which takes as parameters

  * A 2D array representing the flight area. The value of a specific coordinate
   in the matrix represents the number of customers at that location. 
  * A tuple representing the take off location of the drone
  * A tuple representing the landing location of the drone

 This function should then return an integer representing the maximum customers that could be serviced on the optimal flight path.

*Conditions:* 

  * A flight path cannot overlap itself.
  * Any point with a value of '-1' is a no-fly area. The drone cannot fly through it.

(Extra Credit) you can extend the function to include an integer "gas" parameter which indicates the maximum number of spaces the drone can traverse.

```python
#Your function goes here
def optimalFlightPath(input_map, start, end):
    pass

#Example inputs
input_map = [
    [0, 0, 1, 1, 5],
    [1, 1, 1, -1, 7],
    [1, 1, 1, -1, 1],
    [1, 1, 1, -1, 9],
    [5, 1, 1, 1, 0],
]

start = (0,0)
end = (4,4)

x = optimalFlightPath(input_map, start, end)
print x #24

```

2. (Extra extra Credit) Check out this branch and make your own branch. Add a sketch or related image to the branch in the theme of the question above. Commit it back to this repo.

## send your responses directly to shay.strong@eagleview.com. Don't upload your answers here, unless you want to share them with the world.
