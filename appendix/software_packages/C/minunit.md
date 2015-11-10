\paragraph{MinUnit}
As per MinUnit's description, "MinUnit is an extremely simple unit testing framework written in C. It uses no memory allocation, so it should work fine under almost any circumstance, including ROMable code."

```
/* file: minunit.h */
 #define mu_assert(message, test) do { if (!(test)) return message; } while (0)
 #define mu_run_test(test) do { char *message = test(); tests_run++; \
                                if (message) return message; } while (0)
 extern int tests_run;
```

As per MinUnit's description: "No, that's not a typo. It's just 3 lines of code."

MinUnit has also been added to `tools`.

I wanted to be able to add more information to tests being run using the [`minunit.h`](http://www.jera.com/techinfo/jtns/jtn002.html) testing harness. In particular, I wanted to think of a test run as a "test" and an individual assert within that test run as a subtest. I wanted to be able to name each of these and have their status displayed in `STDOUT`. 

\paragraph{My MinUnit Implementation}
```
/* file: my_minunit.h */
#define mu_assert(subtest_desc, test,message) do { \
   if (!(test)) { \
   printf("subtest: \"%s\" FAILED\n", subtest_desc); \
   return message; \
   } \
   else { \
     printf("subtest: \"%s\" PASSED\n", subtest_desc); \
   } \
} while (0)

#define mu_run_test(test_desc,test) do { \
  printf("\nTest: \"%s\"\n",test_desc); \
  char *message = test(); tests_run++; \
  if (message) return message; } while (0)

extern int tests_run;
```

A little less minimal but still small. I also added this file to `/usr/include` so as to not have to tell `gcc` where to find it.

\paragraph{A typical output}
```

Test: "Test of GSL"
y: -0.177597     expected_y: -0.177597
subtest: "value of zero order Bessel function of the first kind" PASSED

Test: "Test of Rectangular Complex Number Struct"
subtest: "real part of a rectangular complex number" PASSED
subtest: "imaginary part of a rectangular complex number" PASSED
subtest: "real part of a rectangular complex number after redefinition" PASSED
subtest: "imaginary part of a rectangular complex number after redefinition" PASSED

All Tests Passed
Tests run: 2
```

\paragraph{Spec file for this output}

```
#include <stdio.h>
#include <gsl/gsl_sf_bessel.h>
#include <gsl/gsl_complex_math.h>
#include <math.h>
#include <my_minunit.h>

int tests_run = 0;
float round_to_6_places(float num);

static 
char * test_gsl_via_0_order_bessel_function_of_the_first_kind ()
{
  double x = 5.0;
  double y = round_to_6_places(gsl_sf_bessel_J0 (x));
  double expected_y = round_to_6_places(-1.775967713143382920e-01);
  printf("y: %f \t expected_y: %f\n",y,expected_y);
  mu_assert
  (
    "value of zero order Bessel function of the first kind",
    y == expected_y,
    "y: not equal to expected_y" 
  );
  return 0;
}

static
char * test_gsl_rectangular_complex_number_struct()
{
  double x = 2.43728;
  double y = 3.23412;

  gsl_complex test_rect_complex_number = gsl_complex_rect ( x, y ); 

  mu_assert
  (
    "real part of a rectangular complex number",
    GSL_REAL(test_rect_complex_number) == x,
    "real part of rectangular complex number does not match expected" 
  );

  mu_assert
  (
    "imaginary part of a rectangular complex number",
    GSL_IMAG(test_rect_complex_number) == y,
    "imaginary part of rectangular complex number does not match expected" 
  );

  GSL_SET_REAL(&test_rect_complex_number,y);
  GSL_SET_IMAG(&test_rect_complex_number,x);
  
  mu_assert
  (
    "real part of a rectangular complex number after redefinition",
    GSL_REAL(test_rect_complex_number) == y,
    "redefined real part of rectangular complex number does not match expected" 
  );

  mu_assert
  (
    "imaginary part of a rectangular complex number after redefinition",
    GSL_IMAG(test_rect_complex_number) == x,
    "redefined imaginary part of rectangular complex number does not match expected" 
  );

  return 0;
}  

static 
char * all_tests ()
{
  mu_run_test("Test of GSL", test_gsl_via_0_order_bessel_function_of_the_first_kind); 
  mu_run_test("Test of Rectangular Complex Number Struct", 
              test_gsl_rectangular_complex_number_struct);
  return 0;
}

int 
main(int argc, char **argv)
{
  char * result = all_tests();
  if (result != 0)
  {
    printf("%s\n", result);
  }
  else
  {
    printf("\nAll Tests Passed\n");
  }
  printf("Tests run: %d\n", tests_run);

  return result != 0;
}

float 
round_to_6_places(float num)
{
  float nearest = roundf(num * 10000000) / 10000000;
  return nearest;
}
```

