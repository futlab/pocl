
#AM_CPPFLAGS = -I$(top_srcdir)/fix-include -I$(top_srcdir)/include -DSRCDIR='"$(abs_srcdir)"'
add_definitions("-DSRCDIR=\"${CMAKE_CURRENT_SOURCE_DIR}\"")

# example1_CFLAGS = @OPENCL_CFLAGS@
#set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c99 ${OPENCL_CFLAGS}")
add_compile_options(${OPENCL_CFLAGS})


add_executable("example1" example1.c example1_exec.c example1.cl)

# example1_LDADD = @OPENCL_LIBS@ ../../lib/poclu/libpoclu.la
target_link_libraries("example1" ${POCLU_LINK_OPTIONS})

add_test("spec_tests/example1_dot_product" "example1")

set_tests_properties( "spec_tests/example1_dot_product"
  PROPERTIES
    COST 40.0
    PASS_REGULAR_EXPRESSION "OK\n"
    ENVIRONMENT "POCL_BUILDING=1"
    PROCESSORS 1
    LABELS "OpenCL_Spec"
    DEPENDS "pocl_version_check")