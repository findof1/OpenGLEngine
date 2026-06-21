set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(glfw3 REQUIRED)

include_directories(
    ${GLFW_INCLUDE_DIRS}
    "${CMAKE_SOURCE_DIR}/External/glad/include"
    "${CMAKE_SOURCE_DIR}/External"
    "${CMAKE_SOURCE_DIR}/Engine/Include"
)

link_directories(
    ${GLFW_LIBRARY_DIRS}
)

file(GLOB ENGINE_SRC "${CMAKE_SOURCE_DIR}/Engine/src/*.cpp")
file(GLOB GLAD_SRC "${CMAKE_SOURCE_DIR}/External/glad/src/*.c")

add_executable(GameEngine
    ${ENGINE_SRC}
    ${GLAD_SRC}
)

#target_compile_definitions(GameEngine PRIVATE NDEBUG)

target_link_libraries(GameEngine
    glfw
)

add_custom_command(TARGET GameEngine POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory
    ${CMAKE_SOURCE_DIR}/Assets $<TARGET_FILE_DIR:GameEngine>/Assets
)

add_custom_command(TARGET GameEngine POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory
    ${CMAKE_SOURCE_DIR}/shaders $<TARGET_FILE_DIR:GameEngine>/shaders
)