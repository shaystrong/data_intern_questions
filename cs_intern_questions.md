'''
You have just received a job in charge of the flight planning system for a drone
delivery service. You're responsible for designing a system that when given a
virtual map of an area can map out a flight path for the drone that will
maximize the number of customers served. 
Problem:
Write a function which takes as parameters
 - A 2D array representing the flight area. The value of a specific coordinate
   in the matrix represents the number of customers at that location. 
 - A tuple representing the take off location of the drone
 - A tuple representing the landing location of the drone
 This function should then return an integer representing the maximum customers
 that could be serviced on the optimal flight path.
Conditions:
- A flight path cannot overlap itself.
- Any point with a value of '-1' is a no-fly area. The drone cannot fly through 
  it.
For extra credit you can extend the function to include an integer "gas"
parameter which indicates the maximum number of spaces the drone can traverse.
'''

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
