# TDT4200_assignment 4

## Task 1

a) Measure the runtime of your application

All measurements are done on a Dell LATITUDE E5450 with a Intel(R) Core(TM) i5-5300U CPU @ 2.30GHz cpu.

make time reports 0:10.22 elapsed / 0:09.95 elapsed / 0:10.06 when running the command three times.

b) Change the parameters and report on measurement differences.

```
time ./mandel/mandel -x 0.5 -y 0.5 -s 1.0 -r 512 -i 512 -c 1 output/mandelnav.bmp
```
Halving the resolution from the baseline seem to cut the time down to 2.543s.

```
time ./mandel/mandel -x 0.15 -y 0.5 -s 1.0 -r 1024 -i 512 -c 1 output/mandelnav.bmp
```
Moving the center from the baseline seem to cut the time to 1.413s. Likely as a result of the calculation being able to determin that the values are outside of the set before reaching the itteration limit or is outside the possible range.

```
time ./mandel/mandel -x 0.5 -y 0.5 -s 0.1 -r 1024 -i 512 -c 1 output/mandelnav.bmp
```
A 10x scale from the baseline seem to increase the time to 25.576s. Probably because the position makes it harder to determin if a value is within the set.

## Task 2 

b) Identify at least two positions in the application which show abad utilization of cache ressource.

By looking at the Last level miss sum, it seem that the worst cach utilization is by mapDwellBuffer and computeDwellBuffer.

|Function 			| Ilmr 	| DLmr  | DLmw  | Last-level miss sum 	|
|-------------------|-------|-------|-------|-----------------------|
|mapDwellBuffer		| 0.49	| 76.53	| 21.58	|	     44.88			|
|computeDwellBuffer	| 0.39	| 0.00	| 56.65	|	     32.43			|

c) Identify 4 functions in the application which are responsible formost of the runtime and document their share of the runtime.

|	function 		|	Self Ir |
|-------------------|-----------|
| getInitialValue	| 25.52		|
| computeNextValue	| 16.22		|
| pixelDwell		| 13.49		|
| isPartOfMendelbrot| 7.43		|

All functions are from main.c that is responsible for 62.97 % of the instruction fetches (Ir). 

e) Measure one of the functions identified in task 2 d.

getInitialValue has a total runtime of about 9.182 sec when it runs 211440966 times as it does in the baseline.

## Task 3 

a) Perform optimizations to im-prove cache performance based on cachegrind

- Changed mapDwellBuffer to itterate over x before next y to use chachlines more effectivly.
- Moved color malloc/free out of forloop. Changed time from about 0.1 to 0.08.
- Changed computeDwellBuffer to itterate over rows all x before next y. No quantifiable difference in time.
- Changed mapDwellBuffer to use memcpy instead of assigning r,g,b individually. Improvement of about 0.005-0.01 second
- Changed pixelDwell to take dwell buffer as an argument rather than returning for assignment.

c)  Now  use  the  findings  from  Callgrind  and  perform  optimizations  on  thedocumented bottlenecks.

- Changed time spent on getInitialValue from 9.182 to 0.064 by saving the old InitialValue instead of recomputing. Calls to function reduced to 1048576 from 211440966.
- Removed computeNextValue and computed locally in pixelDwell. Removing unnecessary function call/ stackframe creation. Changing time of pixelDwell from about 5.8 to 4.6.
- Removed dwellInc and changed dwell+= dwellInc to ++dwell, as dwellInc was always 1 and used nowhere else. Seem to have improved efficiency by about 0.05-0.1 seconds. 
- Removed isPartOfMandelbrot as it was only used one place and changed 2*2 to 4 as it was called 211440966 times. Changed time of pixelDwell from about 4.5 to 3.4.
- Changed getInitialValue to not save return variable before returning. No change in time.