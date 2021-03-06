PROJECT(TinyXML)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/../common/cmake/)
if(USE_LOCAL_TINYXML)
    set(TINYXML_FOUND "NO")
    message(STATUS "Using local TinyXML")
else()
    find_package(TINYXML)
endif()
if (NOT TINYXML_FOUND)
    file (GLOB TinyXML_src src/*.c* src/*.def)
    file (GLOB TinyXML_headers include/*.h*)

    set(TinyXML_HEADERS_DIR ${TinyXML_SOURCE_DIR}/include)

    include_directories(${TinyXML_HEADERS_DIR})

    add_library(tinyxml STATIC ${TinyXML_src} ${TinyXML_headers})

    set_target_properties(tinyxml PROPERTIES COMPILE_FLAGS "${COMPILE_FLAGS}")
    set_property(TARGET tinyxml PROPERTY FOLDER "third_party")
    if (WITH_STATIC)
        pack_static(tinyxml)
    endif()
else()
    set(TinyXML_HEADERS_DIR ${TINYXML_INCLUDE_DIRS})
endif()
