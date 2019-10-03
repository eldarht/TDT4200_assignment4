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