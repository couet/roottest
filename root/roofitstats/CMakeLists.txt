if(ROOT_roofit_FOUND)
  if(ROOT_pyroot_FOUND)
    ROOTTEST_ADD_TEST(read-scientificnotation
                      MACRO read_scientific_notation.py
                      OUTCNVCMD grep -v -e "Wouter"
                      OUTREF read_scientific_notation.ref)
  endif()

  ROOTTEST_ADD_TEST(RooDataSet_ASCII_in
                    MACRO ${CMAKE_CURRENT_SOURCE_DIR}/ASCII-in-out.C
                    MACROARG "false, \"${CMAKE_CURRENT_SOURCE_DIR}/ASCII-in-out_data.txt\"")
if(MSVC AND MSVC_VERSION LESS 1938)
  ROOTTEST_ADD_TEST(RooDataSet_ASCII_out
                    MACRO ${CMAKE_CURRENT_SOURCE_DIR}/ASCII-in-out.C
                    MACROARG "true, \"./ASCII-in-out_result.txt\""
                    ${WILLFAIL_ON_WIN32}
                    POSTCMD diff ${CMAKE_CURRENT_SOURCE_DIR}/ASCII-in-out_data.txt ./ASCII-in-out_result.txt)
else()
  ROOTTEST_ADD_TEST(RooDataSet_ASCII_out
                    MACRO ${CMAKE_CURRENT_SOURCE_DIR}/ASCII-in-out.C
                    MACROARG "true, \"./ASCII-in-out_result.txt\""
                    POSTCMD diff ${CMAKE_CURRENT_SOURCE_DIR}/ASCII-in-out_data.txt ./ASCII-in-out_result.txt)
endif()

  ROOT_ADD_GTEST(stabilityTests stabilityTests.cxx LIBRARIES ROOT::RooFit
                 COPY_TO_BUILDDIR ${CMAKE_CURRENT_SOURCE_DIR}/stabilityTests_data_1.root)

  ROOT_ADD_GTEST(loadOldWorkspace loadOldWorkspace.cxx LIBRARIES ROOT::RooFit
                 COPY_TO_BUILDDIR
                 ${CMAKE_CURRENT_SOURCE_DIR}/rf502_workspace_v5.34.root
                 ${CMAKE_CURRENT_SOURCE_DIR}/rf502_workspace_v6.04.root
                 ${CMAKE_CURRENT_SOURCE_DIR}/rf502_workspace_v6.14.root
                 )

  ROOT_ADD_GTEST(testRooWorkspace testRooWorkspace.cxx LIBRARIES ROOT::RooFit
                 COPY_TO_BUILDDIR
                 ${CMAKE_CURRENT_SOURCE_DIR}/data/test_workspace_01.root
                 )

  add_subdirectory(vectorisedPDFs)
endif()
