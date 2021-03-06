cmake_minimum_required(VERSION 3.8)

#Project config
set(PROJECT_NAME "ip_filter")
if(DEFINED ENV{TRAVIS_BUILD_NUMBER})
  project(${PROJECT_NAME} VERSION 0.0.$ENV{TRAVIS_BUILD_NUMBER})
else()
  project(${PROJECT_NAME} VERSION 0.0.1)
endif()



#Packages
set (GTEST_ROOT /usr/src/gtest)
set (GTEST_LIBRARY /usr/src/gtest/libgtest.a)
set (GTEST_MAIN_LIBRARY /usr/src/gtest/libgtest_main.a)

find_package(Boost REQUIRED)
find_package(Boost COMPONENTS system REQUIRED)
find_package(GTest REQUIRED)

find_package(Threads REQUIRED)

include_directories(
    ../include
    ${GTEST_INCLUDE_DIRS}
)

#Configure
#configure_file(version.h.in version.h)

#Targets
add_executable(ip_filter main.cpp)
#add_library(static_library lib.cpp)

add_executable(input_test input_test.cpp)
add_executable(lexicographical_reverse_test lexicographical_reverse_test.cpp)
add_executable(filtration_by_first_byte_test filtration_by_first_byte_test.cpp)
add_executable(filtration_by_first_and_second_bytes_test filtration_by_first_and_second_bytes_test.cpp)
add_executable(filtration_by_any_byte_test filtration_by_any_byte_test.cpp)



#Compilation setings
set_target_properties(ip_filter 
  input_test 
  lexicographical_reverse_test
  filtration_by_first_byte_test 
  filtration_by_first_and_second_bytes_test
  filtration_by_any_byte_test
PROPERTIES
    CXX_STANDARD 17
    CXX_STANDARD_REQUIRED ON
    COMPILE_OPTIONS "-Wpedantic;-Wall;-Wextra;--std=c++17;-lboost_system;"
    INCLUDE_DIRECTORIES ${Boost_INCLUDE_DIR}
)
set_target_properties(
  input_test 
  lexicographical_reverse_test
  filtration_by_first_byte_test 
  filtration_by_first_and_second_bytes_test
  filtration_by_any_byte_test
PROPERTIES
    INCLUDE_DIRECTORIES ${GTEST_INCLUDE_DIR}
)


#Linking
target_link_libraries(ip_filter
    ${Boost_LIBRARIES}
)


target_link_libraries(input_test
    ${Boost_LIBRARIES}
    ${GTEST_LIBRARIES}
    Threads::Threads
)
target_link_libraries(lexicographical_reverse_test
    ${Boost_LIBRARIES}
    ${GTEST_LIBRARIES}
    Threads::Threads
)
target_link_libraries(filtration_by_first_byte_test
    ${Boost_LIBRARIES}
    ${GTEST_LIBRARIES}
    Threads::Threads
)
target_link_libraries(filtration_by_first_and_second_bytes_test
    ${Boost_LIBRARIES}
    ${GTEST_LIBRARIES}
    Threads::Threads
)
target_link_libraries(filtration_by_any_byte_test
    ${Boost_LIBRARIES}
    ${GTEST_LIBRARIES}
    Threads::Threads
)

#Install
install(TARGETS ip_filter RUNTIME DESTINATION bin)
#CPack
set (CPACK_GENERATOR DEB)

#set (CPACK_DEB_COMPONENT_INSTALL ON)
#set (CPACK_DEBIAN_ENABLE_COMPONENT_DEPENDS ON)
#set (CPACK_DEBIAN_PROG_PACKAGE_DEPENDS ipfilter-lib)
#set (CPACK_DEBIAN_PACKAGE_SHLIBDEPS ON)

set (CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set (CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set (CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")
#set (CPACK_PACKAGE_ARCHITECTURE "amd64")
set(CPACK_PACKAGE_CONTACT vsuvorov1996@gmail.com)
#set (CPACK_DEBIAN_FILE_NAME DEB-DEFAULT)


#set(CPACK_GENERATOR DEB)
#set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
#set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
#set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")
#set(CPACK_PACKAGE_CONTACT vsuvorov1996@gmail.com)
include(CPack)


#Testing
enable_testing()
add_test(input_test input_test)
add_test(lexicographical_reverse_test lexicographical_reverse_test)
add_test(filtration_by_first_byte_test filtration_by_first_byte_test)
add_test(filtration_by_first_and_second_bytes_test filtration_by_first_and_second_bytes_test)
add_test(filtration_by_any_byte_test filtration_by_any_byte_test)
