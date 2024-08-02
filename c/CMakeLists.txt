cmake_minimum_required(VERSION 3.21)
project(c-guide)

############################################
# Prerequisites Tests                      #
############################################
find_program(PDFLATEX NAMES "pdflatex" "pdflatex.exe")
if (NOT PDFLATEX)
    message(FATAL_MESSAGE "A required executable 'pdflatex' is not found. Did you add its directory to environment variable?")
endif ()

############################################
# Build Items                              #
############################################
set(SOURCE "${CMAKE_CURRENT_LIST_DIR}/c.tex")
set(OUTPUT "${CMAKE_BINARY_DIR}/c.pdf")
set(ALL_OUTPUT
    "${CMAKE_BINARY_DIR}/c.aux"
    "${CMAKE_BINARY_DIR}/c.log"
    "${CMAKE_BINARY_DIR}/c.out"
    "${CMAKE_BINARY_DIR}/c.pdf"
    "${CMAKE_BINARY_DIR}/c.toc"
)

############################################
# Build Targets                            #
############################################
add_custom_target(
    c-guide ALL
    COMMAND pdflatex -interaction=nonstopmode -output-directory "${CMAKE_BINARY_DIR}" "${SOURCE}"
    WORKING_DIRECTORY "${CMAKE_CURRENT_LIST_DIR}"
    BYPRODUCTS "${OUTPUT}"
    DEPENDS    "${SOURCE}"
    SOURCES    "${SOURCE}"
)
