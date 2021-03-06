# Path to the Sphinx inventory file of the library
set(TRIQS_INVENTORY_DIR "${TRIQS_PATH}/share/doc/triqs" CACHE STRING
    "Path or URL to a directory containing a Sphinx inventory file of the TRIQS library")
mark_as_advanced(TRIQS_INVENTORY_DIR)

# Path to the Sphinx extensions provided by TRIQS
set(TRIQS_SPHINXEXT_PATH "${TRIQS_PATH}/share/triqs/sphinxext")

# generate the conf.py
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/conf.py.in ${CMAKE_CURRENT_BINARY_DIR}/conf.py)

# all rst files of the documentation
file(GLOB_RECURSE doc_sources *.rst)

# create documentation target
set(sphinx_top ${CMAKE_CURRENT_BINARY_DIR}/html/index.html)
add_custom_command(OUTPUT ${sphinx_top} DEPENDS ${doc_sources} py_copy cthyb cthyb_c cthyb
                   COMMAND ${CMAKE_BINARY_DIR}/build_pytriqs ${TRIQS_SPHINXBUILD_EXECUTABLE} -c . -b html ${CMAKE_CURRENT_SOURCE_DIR} html)
add_custom_target(doc_sphinx ALL DEPENDS ${sphinx_top})

# install
install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/html/ COMPONENT documentation DESTINATION share/doc/cthyb
 FILES_MATCHING 
 PATTERN "*.html" 
 PATTERN "*.png"
 PATTERN "*.js"
 PATTERN "_*"
 PATTERN "*.png" 
 PATTERN "*.jpg" 
 PATTERN "*.gif" 
 PATTERN "*.xsl" 
 PATTERN "*.css"
 PATTERN "*.pdf"
 PATTERN "*.py"
 PATTERN "*.txt"
 PATTERN "*.inv"
 PATTERN "*.bib"
 )
