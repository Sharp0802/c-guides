cmake_minimum_required(VERSION 3.21)
project(c-guide)

############################################
# Prerequisites Tests                      #
############################################
if (NOT DEFINED BROWSER)
    if (NOT DEFINED ENV{BROWSER})
        message(WARNING "Couldn't get browser information; Please set BROWSER environment variable.")
    else ()
        set(BROWSER "$ENV{BROWSER}")
    endif()
endif ()

find_program(PDFLATEX NAMES "pdflatex" "pdflatex.exe")
if (NOT PDFLATEX)
    message(FATAL_MESSAGE "A required executable 'pdflatex' is not found. Did you add its directory to environment variable?")
endif ()

############################################
# Build Items                              #
############################################
set(SOURCE "${CMAKE_CURRENT_LIST_DIR}/c.tex")
set(OUTPUT "${CMAKE_BINARY_DIR}/c.pdf")
set(VERBOSE_OUTPUT
    "${CMAKE_BINARY_DIR}/c.aux"
    "${CMAKE_BINARY_DIR}/c.log"
)

############################################
# Build Targets                            #
############################################
add_custom_target(
    c-guide ALL
    COMMAND pdflatex -interaction=nonstopmode "${SOURCE}"
    WORKING_DIRECTORY "${CMAKE_BINARY_DIR}"
    BYPRODUCTS "${OUTPUT}"
    DEPENDS    "${SOURCE}"
    SOURCES    "${SOURCE}"
)
add_custom_command(
    TARGET c-guide
    POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E rm ${VERBOSE_OUTPUT}
)

if (DEFINED BROWSER)
    add_custom_target(
        preview
        COMMAND ${BROWSER} ${OUTPUT} &
        USES_TERMINAL
        VERBATIM
        DEPENDS c-guide
    )
endif ()
    