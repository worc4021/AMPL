cmake_minimum_required(VERSION 3.25)
project(AMPL)

include(GNUInstallDirs)
include(FetchContent)

FetchContent_Declare(
      asl
      URL https://www.netlib.org/ampl/solvers2.tgz
)
FetchContent_MakeAvailable(asl)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR})
include(ConfigureSource)

set(AMPL_SOURCES  
	${asl_SOURCE_DIR}/asldate.c 
	${asl_SOURCE_DIR}/atof.c 
	${asl_SOURCE_DIR}/auxinfo.c 
	${asl_SOURCE_DIR}/avltree.c 
	${asl_SOURCE_DIR}/b_search.c 
	${asl_SOURCE_DIR}/basename.c 
	${asl_SOURCE_DIR}/bscanf.c 
	${asl_SOURCE_DIR}/conscale.c 
	${asl_SOURCE_DIR}/degree.c 
	${asl_SOURCE_DIR}/derprop.c 
	${asl_SOURCE_DIR}/details.c 
	${asl_SOURCE_DIR}/dtoa1.c 
	${asl_SOURCE_DIR}/duthes.c 
	${asl_SOURCE_DIR}/dynlink.c 
	${asl_SOURCE_DIR}/eval1.c 
	${asl_SOURCE_DIR}/eval2.c 
	${asl_SOURCE_DIR}/ewalloc1.c 
	${asl_SOURCE_DIR}/ewalloc2.c 
	${asl_SOURCE_DIR}/f_read.c 
	${asl_SOURCE_DIR}/fg_read.c 
	${asl_SOURCE_DIR}/fg_write.c 
	${asl_SOURCE_DIR}/fpecatch.c 
	${asl_SOURCE_DIR}/fpinit.c 
	${asl_SOURCE_DIR}/fullhes.c 
	${asl_SOURCE_DIR}/func_add.c 
	${asl_SOURCE_DIR}/funcadd1.c 
	${asl_SOURCE_DIR}/g_fmt.c 
	${asl_SOURCE_DIR}/genrowno.c 
	${asl_SOURCE_DIR}/getenv.c 
	${asl_SOURCE_DIR}/getstub.c 
	${asl_SOURCE_DIR}/htcl.c 
	${asl_SOURCE_DIR}/indic_cons.c 
	${asl_SOURCE_DIR}/jac0dim.c 
	${asl_SOURCE_DIR}/jacdim.c 
	${asl_SOURCE_DIR}/jacinc.c 
	${asl_SOURCE_DIR}/jacinc1.c 
	${asl_SOURCE_DIR}/libnamsave.c 
	${asl_SOURCE_DIR}/mach.c 
	${asl_SOURCE_DIR}/mainexit.c 
	${asl_SOURCE_DIR}/mip_pri.c 
	${asl_SOURCE_DIR}/misc.c 
	${asl_SOURCE_DIR}/mpec_adj.c 
	${asl_SOURCE_DIR}/mqpcheckv.c 
	${asl_SOURCE_DIR}/mypow.c 
	${asl_SOURCE_DIR}/names.c 
	${asl_SOURCE_DIR}/nl_obj.c 
	${asl_SOURCE_DIR}/nqpcheck.c 
	${asl_SOURCE_DIR}/nqpcheckZ.c 
	${asl_SOURCE_DIR}/obj_adj.c 
	${asl_SOURCE_DIR}/obj_prec.c 
	${asl_SOURCE_DIR}/objconst.c 
	${asl_SOURCE_DIR}/objval_.c 
	${asl_SOURCE_DIR}/op_type.c 
	${asl_SOURCE_DIR}/pfghread.c 
	${asl_SOURCE_DIR}/printf.c 
	${asl_SOURCE_DIR}/pshvprod.c 
	${asl_SOURCE_DIR}/punknown.c 
	${asl_SOURCE_DIR}/qpcheck.c 
	${asl_SOURCE_DIR}/qpcheckZ.c 
	${asl_SOURCE_DIR}/qsortv.c 
	${asl_SOURCE_DIR}/readsol.c 
	${asl_SOURCE_DIR}/repwhere.c 
	${asl_SOURCE_DIR}/sigcatch.c 
	${asl_SOURCE_DIR}/sos_add.c 
	${asl_SOURCE_DIR}/sphes.c 
	${asl_SOURCE_DIR}/sscanf.c 
	${asl_SOURCE_DIR}/stderr.c 
	${asl_SOURCE_DIR}/studchk0.c 
	${asl_SOURCE_DIR}/suf_sos.c 
	${asl_SOURCE_DIR}/value.c 
	${asl_SOURCE_DIR}/writesol.c 
	${asl_SOURCE_DIR}/wrtsol_.c 
	${asl_SOURCE_DIR}/ws_desc.c 
	${asl_SOURCE_DIR}/wsu_desc.c 
	${asl_SOURCE_DIR}/xectim.c 
	${asl_SOURCE_DIR}/xp2known.c)

# if(WIN32)
#       list(APPEND AMPL_SOURCES ${asl_SOURCE_DIR}/fpinitmt.c)
# else()
#       list(APPEND AMPL_SOURCES ${asl_SOURCE_DIR}/fpinit.c)
# endif()


add_library(ampl ${AMPL_SOURCES})

file(GLOB ASL_HEADERS "${asl_SOURCE_DIR}/*.h")

target_sources(ampl PRIVATE 
				FILE_SET HEADERS
				BASE_DIRS ${asl_SOURCE_DIR}
				FILES ${ASL_HEADERS})

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