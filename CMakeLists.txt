cmake_minimum_required(VERSION 3.12)
project(Papers LANGUAGES NONE VERSION 0.0.0)

include(pmm.cmake)

pmm(CMakeCM ROLLING)

find_package(Bikeshed REQUIRED)

function(add_paper number)
    get_filename_component(in_file "${number}.bs" ABSOLUTE)
    get_filename_component(out_file "${CMAKE_CURRENT_BINARY_DIR}/${number}.html" ABSOLUTE)
    add_custom_command(
        OUTPUT "${out_file}"
        DEPENDS "${in_file}"
        COMMAND Bikeshed::Bikeshed spec "${in_file}" "${out_file}"
        COMMENT "Rendering paper ${number}"
        )
    add_custom_target(paper-${number} ALL DEPENDS "${out_file}")
    add_custom_target(paper-${number}-watch
        COMMAND Bikeshed::Bikeshed watch "${in_file}" "${out_file}"
        COMMENT "Watching ${in_file} for changes"
        USES_TERMINAL
        )
endfunction()

add_paper(pipeline-rewrite)
