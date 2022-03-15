cmake_minimum_required(VERSION 3.12.0)
cmake_minimum_required(VERSION 3.12.0)

add_definitions(-DSQLITE_ENABLE_RTREE)
add_definitions(-DSQLITE_ENABLE_FTS4)
add_definitions(-DSQLITE_ENABLE_FTS5)
add_definitions(-DSQLITE_ENABLE_JSON1)
add_definitions(-DSQLITE_ENABLE_RBU)
add_definitions(-DSQLITE_ENABLE_STAT4)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)

add_library(sqlite3 STATIC src/sqlite3.c)
add_executable(sqlite ${sqlite3}
                      src/shell.c)


if(WIN32)
  add_custom_command(TARGET sqlite POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy $<TARGET_FILE:sqlite> ${CMAKE_BINARY_DIR}/sqlite3.exe
    DEPENDS sqlite
  )
  install(FILES sqlite3.exe DESTINATION bin)
else()
  target_link_libraries(sqlite3 pthread dl)
  target_link_libraries(sqlite  sqlite3
                                pthread
                                dl
                                m)
  install(TARGETS sqlite RUNTIME DESTINATION bin)
endif()

install(
    DIRECTORY "${CMAKE_SOURCE_DIR}/include/"
    DESTINATION "include/"
    )
install(TARGETS sqlite3 LIBRARY DESTINATION lib ARCHIVE DESTINATION lib)
