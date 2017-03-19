cmake_minimum_required(VERSION 3.8.0)

project(data_migration_study)

if (EXISTS $ENV{GOOGLE_TEST_DIR})
	include_directories($ENV{GOOGLE_TEST_DIR}/include)
	add_library(gtest SHARED IMPORTED)
	set_property(TARGET gtest PROPERTY IMPORTED_LOCATION $ENV{GOOGLE_TEST_DIR}lib/gtest.dll)
	set_property(TARGET gtest PROPERTY IMPORTED_IMPLIB $ENV{GOOGLE_TEST_DIR}lib/gtest.lib)
	set(EXTRA_TEST_LIBS ${EXTRA_TEST_LIBS} gtest)
else()
	message("Cannot find google test library.")
	message("Please specify the path and re-run the command like GOOGLE_TEST_DIR=<HOME>/googletest cmake .")
endif ()

add_executable(data_migration_study_test test/main.cpp)
target_link_libraries(data_migration_study_test ${EXTRA_TEST_LIBS})
add_custom_command(TARGET data_migration_study_test POST_BUILD COMMAND ${CMAKE_COMMAND} -E copy_if_different $ENV{GOOGLE_TEST_DIR}lib/gtest.dll $<TARGET_FILE_DIR:data_migration_study_test>)
