\paragraph{GSL}
Central to this work is the [GNU Scientific Library](http://www.gnu.org/software/gsl/) which offers implementations of `openblas` (Open Basic Linear Algebra System) and `lapack` (Linear Algebra Package) necessary for the completion of this work. We have used `homebrew` to install `gsl` and `pkg-config` 

```
$ brew install pkg-config
$ brew install gsl
$ pkg-config --libs gsl
-L/usr/local/Cellar/gsl/1.16/lib -lgsl -lgslcblas -lm
```

and then added the linker statements to our makefile:

```
#Tool Definitions
CC=gcc
CFLAGS=-I. -I$(PATHU) -DTEST
CFLAGS+=-I/usr/local/opt/openblas/include
CFLAGS+=-lgsl -lgslcblas -lm
```

\paragraph{Known Eigenvalues}
We have written a short python script in order to calculate a few known eigenvalues, in this case for the 4th order Hibert Matrix.

```
import numpy
import scipy
from scipy.linalg import *

h4 = hilbert(4)
eigs_h4 = eig(h4)
print eigs_h4
```

\paragraph{Test-Driven Development}
We begin the project with a simple tests to verify the known eigenvalues we have previously generated. 

```
#include <stdio.h>
#include <math.h>
#include <gsl/gsl_math.h>
#include <gsl/gsl_eigen.h>
#include "minunit.h"

#define DIMENSION_OF(a) (sizeof(a)/sizeof(a[0]))

gsl_vector * symmetric_eigenvalues(double * data, int return_values);

float round_to_5_places(float num);

int tests_run = 0;

double expected_eigenvalues[] = {1.500214,0.169141,0.006738,0.000097};

double fourth_order_hilbert[] =
{
  1.0  , 1/2.0, 1/3.0, 1/4.0,
  1/2.0, 1/3.0, 1/4.0, 1/5.0,
  1/3.0, 1/4.0, 1/5.0, 1/6.0,
  1/4.0, 1/5.0, 1/6.0, 1/7.0
};

static char * test_find_eigenvalues()
{
  gsl_vector * actual_eigenvalues = symmetric_eigenvalues(fourth_order_hilbert,0);

  for (int i = 0; i < DIMENSION_OF(expected_eigenvalues); i++)
  {
     float expected = expected_eigenvalues[i];
     float actual = round_to_5_places(gsl_vector_get(actual_eigenvalues, i));

     printf("\nexpected: %f actual: %f\n",
       expected,actual);
     mu_assert("error, eigenvalues do not match",
         expected == actual);
  }
  return 0;
}

static char * all_tests()
{
  mu_run_test(test_find_eigenvalues);
  return 0;
}

int main(int argc, char **argv)
{
  char * result = all_tests();
  if (result != 0)
  {
    printf("%s\n", result);
  }
  else
  {
    printf("All Tests Passed\n");
  }
  printf("Tests run: %d\n", tests_run);

  return result != 0;
}

gsl_vector * symmetric_eigenvalues(double * data, int return_values)
{
  gsl_matrix_view my_matrix = gsl_matrix_view_array (data, 4, 4);

  gsl_vector * my_evals = gsl_vector_alloc (4);
  gsl_matrix * my_evecs = gsl_matrix_alloc (4, 4);

  gsl_eigen_symmv_workspace * my_workspace = gsl_eigen_symmv_alloc (4);

  gsl_eigen_symmv (&my_matrix.matrix, my_evals, my_evecs, my_workspace);

  gsl_eigen_symmv_free (my_workspace);

  gsl_eigen_symmv_sort (my_evals, my_evecs, GSL_EIGEN_SORT_ABS_DESC);

  return my_evals;
}

float round_to_5_places(float num)
{
  float nearest = roundf(num * 1000000) / 1000000;
  return nearest;
}
```

\paragraph{Comments and Further Consideration}
I am not pleased with this particular test of the Symmetic Eigensolver implemented in GSL. It is very "numerical". I think a better test would be more mathematical, such as a test of any matrix and the eigenequation itself, $Ax=\lambda x$. I did, however, confirm that MinUnit is an excellent way to begin to implement a TDD mindset in C development. I think that the next step is to begin to write more tests around the openBlas implementation in the GSL.

