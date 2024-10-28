cmake_minimum_required(VERSION 3.21)
project(AMPL)

include(GNUInstallDirs)

set(AMPL_SOURCES  src/asldate.c 
                  src/atof.c 
                  src/auxinfo.c 
                  src/avltree.c 
                  src/b_search.c 
                  src/basename.c 
                  src/bscanf.c 
                  src/com2eval.c 
                  src/comeval.c 
                  src/con1ival.c 
                  src/con2ival.c 
                  src/con2val.c 
                  src/conadj.c 
                  src/conpval.c 
                  src/conscale.c 
                  src/conval.c 
                  src/degree.c 
                  src/derprop.c 
                  src/details.c 
                  src/dtoa1.c 
                  src/duthes.c 
                  src/dynlink.c 
                  src/f_read.c 
                  src/fg_read.c 
                  src/fg_write.c 
                  src/fgh_read.c 
                  src/fpecatch.c 
                  src/fullhes.c 
                  src/func_add.c 
                  src/funcadd1.c 
                  src/g_fmt.c 
                  src/genrowno.c 
                  src/getenv.c 
                  src/getstub.c 
                  src/htcl.c 
                  src/indic_cons.c 
                  src/jac0dim.c 
                  src/jac2dim.c 
                  src/jacdim.c 
                  src/jacinc.c 
                  src/jacinc1.c 
                  src/libnamsave.c 
                  src/mach.c 
                  src/mainexit.c 
                  src/mip_pri.c 
                  src/misc.c 
                  src/mpec_adj.c 
                  src/mqpcheckv.c 
                  src/mypow.c 
                  src/names.c 
                  src/nl_obj.c 
                  src/nqpcheck.c 
                  src/nqpcheckZ.c 
                  src/obj2val.c 
                  src/obj_adj.c 
                  src/obj_prec.c 
                  src/objconst.c 
                  src/objval.c 
                  src/objval_.c 
                  src/op_type.c 
                  src/pfg_read.c 
                  src/pfghread.c 
                  src/printf.c 
                  src/pshvprod.c 
                  src/punknown.c 
                  src/qp_read.c 
                  src/qpcheck.c 
                  src/qpcheckZ.c 
                  src/qsortv.c 
                  src/readsol.c 
                  src/repwhere.c 
                  src/rops.c 
                  src/rops2.c 
                  src/sigcatch.c 
                  src/sos_add.c 
                  src/sphes.c 
                  src/sscanf.c 
                  src/stderr.c 
                  src/studchk0.c 
                  src/suf_sos.c 
                  src/value.c 
                  src/writesol.c 
                  src/wrtsol_.c 
                  src/ws_desc.c 
                  src/wsu_desc.c 
                  src/x2check.c 
                  src/xectim.c 
                  src/xp1known.c 
                  src/xp2known.c)

if(WIN32)
      list(APPEND AMPL_SOURCES src/fpinitmt.c)
else()
      list(APPEND AMPL_SOURCES src/fpinit.c)
endif()


add_library(ampl ${AMPL_SOURCES})

target_include_directories(ampl PUBLIC  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
                                        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/ampl>)

set_target_properties(ampl PROPERTIES 
                        POSITION_INDEPENDENT_CODE ON
                        MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")

file(GLOB AMPL_HEADERS "src/*.h")

install(TARGETS ampl
      EXPORT AMPLTargets
      LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}/${CMAKE_BUILD_TYPE}
      ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}/${CMAKE_BUILD_TYPE}
      RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}/${CMAKE_BUILD_TYPE})
install(FILES ${AMPL_HEADERS} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/ampl)
install(EXPORT AMPLTargets
      FILE AMPLTargets.cmake
      NAMESPACE ampl::
      DESTINATION cmake)