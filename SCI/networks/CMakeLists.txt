set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

macro (add_network_OT _name)
	add_executable(${_name}-OT "main_${_name}.cpp")
    target_link_libraries(${_name}-OT SCI-OT) 
    target_compile_options(${_name}-OT PUBLIC "-Wno-parentheses-equality")
endmacro()

macro (add_network_OT_GEOM _name)
	add_executable(${_name}-OT-GEOM "main_${_name}.cpp")
	target_link_libraries(${_name}-OT-GEOM SCI-OT-GEOM)
	target_compile_options(${_name}-OT-GEOM PUBLIC "-Wno-parentheses-equality")
endmacro()

# Networks
add_network_OT(resnet50)
add_network_OT_GEOM(resnet50)
add_network_OT(densenet121)
add_network_OT_GEOM(densenet121)
