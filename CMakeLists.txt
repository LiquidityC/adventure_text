cmake_minimum_required(VERSION 3.7)

project(adventure_text LANGUAGES CXX VERSION 0.1.0)

set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -Wall -fsanitize=address -fno-omit-frame-pointer")
set(CMAKE_LINKER_FLAGS_DEBUG "${CMAKE_LINKER_FLAGS_DEBUG} -fsanitize=address -fno-omit-frame-pointer")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -O2")

# Include and build SDL3 (static linking)
set(SDL_STATIC ON)
set(SDL_SHARED OFF)
add_subdirectory(lib/SDL)
set(SDL_LIBRARIES SDL3-static SDL3_Headers)

# Include "Dear ImGui" into the build
file(GLOB IMGUI_SRC CONFIGURE_DEPENDS lib/imgui/*.cpp)
list(APPEND IMGUI_SRC
	lib/imgui/backends/imgui_impl_sdl3.cpp
	lib/imgui/backends/imgui_impl_sdlrenderer3.cpp
	)
set(IMGUI_INCLUDE_DIRS lib/imgui lib/imgui/backends)

add_executable(${PROJECT_NAME}
	${IMGUI_SRC}
	lib/imgui/backends/imgui_impl_sdl3
	src/main.cpp
	)
target_include_directories(${PROJECT_NAME} PRIVATE
	${SDL_INCLUDE_DIRS}
	${IMGUI_INCLUDE_DIRS}
	include
	)
target_link_libraries(${PROJECT_NAME} ${SDL_LIBRARIES})
