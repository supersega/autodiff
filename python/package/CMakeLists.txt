# Configure the setup.py file
configure_file(setup.py.in ${CMAKE_CURRENT_BINARY_DIR}/setup.py)
message(STATUS "Current binary dir: ${CMAKE_CURRENT_BINARY_DIR}")

# Create a custom target to build the python package during build stage
add_custom_target(autodiff-setuptools ALL
    DEPENDS autodiff4py
    COMMAND ${CMAKE_COMMAND} -E copy_directory ${CMAKE_CURRENT_SOURCE_DIR}/autodiff ${CMAKE_CURRENT_BINARY_DIR}/autodiff
    COMMAND ${CMAKE_COMMAND} -E copy $<TARGET_FILE:autodiff4py> ${CMAKE_CURRENT_BINARY_DIR}/autodiff
    COMMAND pip install . --force-reinstall
    COMMAND ${CMAKE_COMMAND} -E remove_directory ${CMAKE_CURRENT_BINARY_DIR}/autodiff/$<TARGET_FILE_NAME:autodiff4py>
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
