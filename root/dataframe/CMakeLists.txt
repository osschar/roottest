if( CMAKE_SIZEOF_VOID_P EQUAL 8 )

ROOTTEST_ADD_TESTDIRS()

# Workaround (k)ubuntu bug: runtime issue when using pthread and gcc48
if(ROOTTEST_OS_ID MATCHES Ubuntu)
  set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -Wl,--no-as-needed")
endif()

set(DFLIBRARIES Core RIO Hist Tree MathCore TreePlayer ROOTDataFrame)

# Linked to ROOT-9773: train cache with TTreeReader
ROOTTEST_ADD_TEST(test_trainCache
                  COPY_TO_BUILDDIR bigFile.root
                  MACRO test_trainCache.C
                  OUTREF test_trainCache.ref)
# ROOT-9628
ROOTTEST_ADD_TEST(test_chainZombieFile
                  COPY_TO_BUILDDIR input1.root input2.root
                  MACRO test_chainZombieFile.C)

# ROOT-9366
# We use this to write something like the FCC data model, dictionaries done with aclic
ROOTTEST_ADD_TEST(writeFcc
                  MACRO writeFcc.C+)

# We build and run an executable that reads the file created above without dictionaries
ROOTTEST_GENERATE_EXECUTABLE(test_readFcc readFcc.C LIBRARIES ${DFLIBRARIES})

ROOTTEST_ADD_TEST(test_readFcc
                  EXEC ./test_readFcc
                  COPY_TO_BUILDDIR fccSkim.root
                  DEPENDS ${GENERATE_EXECUTABLE_TEST} writeFcc)

# ROOT-9116
ROOTTEST_GENERATE_EXECUTABLE(test_gdirectoryRestore test_gdirectoryRestore.cxx LIBRARIES ${DFLIBRARIES})
ROOTTEST_ADD_TEST(test_gdirectoryRestore
                  EXEC ./test_gdirectoryRestore
                  DEPENDS ${GENERATE_EXECUTABLE_TEST})

ROOTTEST_ADD_TEST(templateRecursionLimit
                  MACRO test_templateRecursionLimit.C)

ROOTTEST_ADD_TEST(missingBranches
                  MACRO test_missingBranches.C
                  ERRREF test_missingBranches.eref)

if(NOT CMAKE_BUILD_TYPE STREQUAL "Debug")
ROOTTEST_GENERATE_EXECUTABLE(test_hugeRDF test_hugeRDF.cxx
                             COMPILE_FLAGS "-O2"  # too slow otherwise
                             LIBRARIES ${DFLIBRARIES})

ROOTTEST_ADD_TEST(test_hugeRDF
                  EXEC ./test_hugeRDF
                  LABELS longtest
                  DEPENDS ${GENERATE_EXECUTABLE_TEST})
endif()

ROOTTEST_GENERATE_EXECUTABLE(test_snapshot_manytasks test_snapshot_manytasks.cxx LIBRARIES ${DFLIBRARIES})
ROOTTEST_ADD_TEST(test_snapshot_manytasks
                  EXEC ./test_snapshot_manytasks
                  DEPENDS ${GENERATE_EXECUTABLE_TEST})

ROOTTEST_GENERATE_EXECUTABLE(test_columnoverride test_columnoverride.cxx LIBRARIES ${DFLIBRARIES})
ROOTTEST_ADD_TEST(test_columnoverride
                  EXEC ./test_columnoverride
                  DEPENDS ${GENERATE_EXECUTABLE_TEST})

ROOTTEST_GENERATE_EXECUTABLE(regression_emptysource regression_emptysource.cxx LIBRARIES ${DFLIBRARIES})
ROOTTEST_ADD_TEST(regression_emptysource
                  EXEC ./regression_emptysource
                  DEPENDS ${GENERATE_EXECUTABLE_TEST})

ROOTTEST_ADD_TEST(test_snapshotNFiles
                  MACRO test_snapshotNFiles.C)

ROOTTEST_ADD_TEST(test_listFilesCtor
                  MACRO test_listFilesCtor.C)

ROOTTEST_GENERATE_EXECUTABLE(emptysource test_emptysource.cxx LIBRARIES ${DFLIBRARIES})
ROOTTEST_ADD_TEST(emptysource
                  EXEC ./emptysource
                  OUTREF test_emptysource.ref
                  DEPENDS ${GENERATE_EXECUTABLE_TEST})

ROOTTEST_ADD_TEST(regression_snapshot
                  MACRO regression_snapshot.C+
                  OUTREF regression_snapshot.ref)

ROOTTEST_GENERATE_EXECUTABLE(test_inference test_inference.cxx LIBRARIES ${DFLIBRARIES})
ROOTTEST_ADD_TEST(test_inference
                  EXEC ./test_inference
                  OUTREF test_inference.ref
                  DEPENDS ${GENERATE_EXECUTABLE_TEST})

ROOTTEST_ADD_TEST(test_snapshot
                  MACRO test_snapshot.C+
                  OUTREF test_snapshot.ref)

ROOTTEST_ADD_TEST(test_stringfiltercolumn
                  MACRO test_stringfiltercolumn.C+
                  OUTREF test_stringfiltercolumn.ref)

ROOTTEST_ADD_TEST(test_glob
                  MACRO test_glob.C+
                  OUTREF test_glob.ref)

ROOTTEST_ADD_TEST(test_reduce
                  MACRO test_reduce.C+
                  OUTREF test_reduce.ref)

ROOTTEST_GENERATE_EXECUTABLE(test_callables test_callables.cxx LIBRARIES ${DFLIBRARIES})
ROOTTEST_ADD_TEST(test_callables
                  EXEC ./test_callables
                  OUTREF test_callables.ref
                  DEPENDS ${GENERATE_EXECUTABLE_TEST})

ROOTTEST_GENERATE_EXECUTABLE(testIMT testIMT.cxx LIBRARIES ${DFLIBRARIES})
ROOTTEST_ADD_TEST(testIMT
                  EXEC ./testIMT
                  OUTREF testIMT.ref
                  DEPENDS ${GENERATE_EXECUTABLE_TEST})

ROOTTEST_ADD_TEST(test_splitcoll_arrayview
                  MACRO test_splitcoll_arrayview.C+
                  OUTREF test_splitcoll_arrayview.ref)

ROOTTEST_GENERATE_EXECUTABLE(test_readerarray test_readerarray.cxx LIBRARIES ${DFLIBRARIES})
ROOTTEST_ADD_TEST(test_readerarray
                  EXEC ./test_readerarray
                  OUTREF test_readerarray.ref
                  DEPENDS ${GENERATE_EXECUTABLE_TEST})

ROOTTEST_GENERATE_EXECUTABLE(typeguessing test_typeguessing.cxx LIBRARIES ${DFLIBRARIES})
ROOTTEST_ADD_TEST(typeguessing
                  EXEC ./typeguessing
                  OUTREF test_typeguessing.ref
                  DEPENDS ${GENERATE_EXECUTABLE_TEST})

ROOTTEST_GENERATE_EXECUTABLE(misc test_misc.cxx LIBRARIES ${DFLIBRARIES})
ROOTTEST_ADD_TEST(misc
                  EXEC ./misc
                  OUTREF test_misc.ref
                  DEPENDS ${GENERATE_EXECUTABLE_TEST})

ROOTTEST_ADD_TEST(ctors
                  MACRO test_ctors.C+
                  OUTREF test_ctors.ref)

ROOTTEST_GENERATE_EXECUTABLE(branchoverwrite test_branchoverwrite.cxx LIBRARIES ${DFLIBRARIES})
ROOTTEST_ADD_TEST(branchoverwrite
                  EXEC ./branchoverwrite
                  OUTREF test_branchoverwrite.ref
                  DEPENDS ${GENERATE_EXECUTABLE_TEST})

ROOTTEST_GENERATE_EXECUTABLE(regression_multipletriggerrun regression_multipletriggerrun.cxx LIBRARIES ${DFLIBRARIES})
ROOTTEST_ADD_TEST(regression_multipletriggerrun
                  EXEC ./regression_multipletriggerrun
                  OUTREF regression_multipletriggerrun.ref
                  DEPENDS ${GENERATE_EXECUTABLE_TEST})

ROOTTEST_GENERATE_EXECUTABLE(regression_zeroentries regression_zeroentries.cxx LIBRARIES ${DFLIBRARIES})
ROOTTEST_ADD_TEST(regression_zeroentries
                  EXEC ./regression_zeroentries
                  DEPENDS ${GENERATE_EXECUTABLE_TEST})


ROOTTEST_GENERATE_EXECUTABLE(foreach test_foreach.cxx LIBRARIES ${DFLIBRARIES})
ROOTTEST_ADD_TEST(foreach
                  EXEC ./foreach
                  OUTREF test_foreach.ref
                  DEPENDS ${GENERATE_EXECUTABLE_TEST})

ROOTTEST_GENERATE_EXECUTABLE(reports test_reports.cxx LIBRARIES ${DFLIBRARIES})
ROOTTEST_ADD_TEST(reports
                  EXEC ./reports
                  OUTREF test_reports.ref
                  DEPENDS ${GENERATE_EXECUTABLE_TEST})

ROOTTEST_GENERATE_EXECUTABLE(par test_par.cxx LIBRARIES ${DFLIBRARIES})
ROOTTEST_ADD_TEST(par
                  EXEC ./par
                  OUTREF test_par.ref
                  DEPENDS ${GENERATE_EXECUTABLE_TEST})

ROOTTEST_GENERATE_EXECUTABLE(read_leaves test_read_leaves.cxx LIBRARIES ${DFLIBRARIES})
ROOTTEST_ADD_TEST(read_leaves
                  EXEC ./read_leaves
                  COPY_TO_BUILDDIR test_read_leaves.h
                  DEPENDS ${GENERATE_EXECUTABLE_TEST})

ROOTTEST_GENERATE_EXECUTABLE(read_leaves_nodict test_read_leaves_nodict.cxx LIBRARIES ${DFLIBRARIES})
ROOTTEST_ADD_TEST(read_leaves_nodict
                  EXEC ./read_leaves_nodict
                  DEPENDS ${GENERATE_EXECUTABLE_TEST} read_leaves)

ROOTTEST_ADD_TEST(test_readTotemNtuple
                  MACRO test_readTotemNtuple.C
                  COPY_TO_BUILDDIR Slimmed_TotemNTuple_9883.040.ntuple.root
                  OUTREF test_readTotemNtuple.ref)

ROOTTEST_GENERATE_EXECUTABLE(test_progressiveCSV test_progressiveCSV.cxx LIBRARIES ${DFLIBRARIES})
ROOTTEST_ADD_TEST(test_progressiveCSV
                  EXEC ./test_progressiveCSV
                  COPY_TO_BUILDDIR test_progressiveCSV.csv
                  DEPENDS ${GENERATE_EXECUTABLE_TEST})

endif() # CMAKE_SIZEOF_VOID_P EQUAL 4
