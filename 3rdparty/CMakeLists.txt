set(CMAKE_CXX_FLAGS "/nologo")
set(CMAKE_C_FLAGS "/nologo")
set(CMAKE_CXX_FLAGS_RELWITHDEBINFO "/Zi")
set(CMAKE_C_FLAGS_RELWITHDEBINFO "/Zi")

set(G3D "${PROJECT_SOURCE_DIR}/Client/Rendering/g3d")

file(GLOB OPENRBX_GRAPHICS3D_SOURCES "${G3D}/g3dcpp/*.cpp" "${G3D}/ijg/*.c" "${G3D}/png/*.c")
file(GLOB OPENRBX_GLG3D_SOURCES "${G3D}/glg3dcpp/*.cpp" "${G3D}/zlib/*.c")

add_library(graphics3d STATIC ${OPENRBX_GRAPHICS3D_SOURCES})
add_library(glg3d STATIC ${OPENRBX_GLG3D_SOURCES})

foreach(name graphics3d glg3d)
  target_include_directories(${name} PRIVATE
    "${G3D}/include" "${G3D}/png" "${G3D}/zlib" "${G3D}/ijg"
    "${PROJECT_SOURCE_DIR}/Client/Rendering/SDL-1.2.6/include"
  )
  target_compile_definitions(${name} PRIVATE WIN32 NDEBUG _LIB G3D_BUILDING_LIBRARY_DLL _VC80_UPGRADE=0x0710 _MBCS)
  target_compile_options(${name} PRIVATE /O2 /Ob1 /GF /EHsc /Gy)
endforeach()
target_compile_options(graphics3d PRIVATE /W2)
target_compile_options(glg3d PRIVATE /W3)

set(BOOST "${PROJECT_SOURCE_DIR}/Client/boost_1_34_1")

add_library(boost_signals STATIC
  "${BOOST}/libs/signals/src/connection.cpp"
  "${BOOST}/libs/signals/src/named_slot_map.cpp"
  "${BOOST}/libs/signals/src/signal_base.cpp"
  "${BOOST}/libs/signals/src/slot.cpp"
  "${BOOST}/libs/signals/src/trackable.cpp"
)

add_library(boost_thread STATIC
  "${BOOST}/libs/thread/src/barrier.cpp"
  "${BOOST}/libs/thread/src/condition.cpp"
  "${BOOST}/libs/thread/src/exceptions.cpp"
  "${BOOST}/libs/thread/src/mutex.cpp"
  "${BOOST}/libs/thread/src/once.cpp"
  "${BOOST}/libs/thread/src/recursive_mutex.cpp"
  "${BOOST}/libs/thread/src/thread.cpp"
  "${BOOST}/libs/thread/src/tss_dll.cpp"
  "${BOOST}/libs/thread/src/tss_hooks.cpp"
  "${BOOST}/libs/thread/src/tss_pe.cpp"
  "${BOOST}/libs/thread/src/tss.cpp"
  "${BOOST}/libs/thread/src/xtime.cpp"
)
target_compile_definitions(boost_thread PRIVATE BOOST_THREAD_BUILD_LIB=1)

add_library(boost_date_time STATIC
  "${BOOST}/libs/date_time/src/gregorian/greg_month.cpp"
  "${BOOST}/libs/date_time/src/gregorian/greg_weekday.cpp"
  "${BOOST}/libs/date_time/src/gregorian/date_generators.cpp"
)
target_compile_definitions(boost_date_time PRIVATE DATE_TIME_INLINE BOOST_DATE_TIME_STATIC_LINK)

set_target_properties(boost_signals PROPERTIES OUTPUT_NAME "libboost_signals-vc80-mt-1_34_1")
set_target_properties(boost_thread  PROPERTIES OUTPUT_NAME "libboost_thread-vc80-mt-1_34_1")
set_target_properties(boost_date_time PROPERTIES OUTPUT_NAME "libboost_date_time-vc80-mt-1_34_1")

foreach(name boost_signals boost_thread boost_date_time)
  target_include_directories(${name} PRIVATE "${BOOST}/src")
  target_compile_definitions(${name} PRIVATE NDEBUG BOOST_ALL_NO_LIB=1)
  target_compile_options(${name} PRIVATE /Zc:forScope /Zc:wchar_t /O2 /Ob2 /W3 /EHs /GR /wd4675)
endforeach()

set(MESA "${PROJECT_SOURCE_DIR}/Client/Rendering/Mesa-6.5")

set(OPENRBX_MESA_CORE_SOURCES
  "${MESA}/src/mesa/array_cache/ac_context.c"
  "${MESA}/src/mesa/array_cache/ac_import.c"
  "${MESA}/src/mesa/glapi/glapi.c"
  "${MESA}/src/mesa/glapi/glthread.c"
  "${MESA}/src/mesa/main/accum.c"
  "${MESA}/src/mesa/main/api_arrayelt.c"
  "${MESA}/src/mesa/main/api_loopback.c"
  "${MESA}/src/mesa/main/api_noop.c"
  "${MESA}/src/mesa/main/api_validate.c"
  "${MESA}/src/mesa/main/attrib.c"
  "${MESA}/src/mesa/main/blend.c"
  "${MESA}/src/mesa/main/bufferobj.c"
  "${MESA}/src/mesa/main/buffers.c"
  "${MESA}/src/mesa/main/clip.c"
  "${MESA}/src/mesa/main/colortab.c"
  "${MESA}/src/mesa/main/context.c"
  "${MESA}/src/mesa/main/convolve.c"
  "${MESA}/src/mesa/main/debug.c"
  "${MESA}/src/mesa/main/depth.c"
  "${MESA}/src/mesa/main/depthstencil.c"
  "${MESA}/src/mesa/main/dispatch.c"
  "${MESA}/src/mesa/main/dlist.c"
  "${MESA}/src/mesa/main/drawpix.c"
  "${MESA}/src/mesa/main/enable.c"
  "${MESA}/src/mesa/main/enums.c"
  "${MESA}/src/mesa/main/eval.c"
  "${MESA}/src/mesa/main/execmem.c"
  "${MESA}/src/mesa/main/extensions.c"
  "${MESA}/src/mesa/main/fbobject.c"
  "${MESA}/src/mesa/main/feedback.c"
  "${MESA}/src/mesa/main/fog.c"
  "${MESA}/src/mesa/main/framebuffer.c"
  "${MESA}/src/mesa/main/get.c"
  "${MESA}/src/mesa/main/getstring.c"
  "${MESA}/src/mesa/main/hash.c"
  "${MESA}/src/mesa/main/hint.c"
  "${MESA}/src/mesa/main/histogram.c"
  "${MESA}/src/mesa/main/image.c"
  "${MESA}/src/mesa/main/imports.c"
  "${MESA}/src/mesa/main/light.c"
  "${MESA}/src/mesa/main/lines.c"
  "${MESA}/src/mesa/main/matrix.c"
  "${MESA}/src/mesa/main/mm.c"
  "${MESA}/src/mesa/main/occlude.c"
  "${MESA}/src/mesa/main/pixel.c"
  "${MESA}/src/mesa/main/points.c"
  "${MESA}/src/mesa/main/polygon.c"
  "${MESA}/src/mesa/main/rastpos.c"
  "${MESA}/src/mesa/main/renderbuffer.c"
  "${MESA}/src/mesa/main/state.c"
  "${MESA}/src/mesa/main/stencil.c"
  "${MESA}/src/mesa/main/texcompress.c"
  "${MESA}/src/mesa/main/texcompress_fxt1.c"
  "${MESA}/src/mesa/main/texcompress_s3tc.c"
  "${MESA}/src/mesa/main/texenvprogram.c"
  "${MESA}/src/mesa/main/texformat.c"
  "${MESA}/src/mesa/main/teximage.c"
  "${MESA}/src/mesa/main/texobj.c"
  "${MESA}/src/mesa/main/texrender.c"
  "${MESA}/src/mesa/main/texstate.c"
  "${MESA}/src/mesa/main/texstore.c"
  "${MESA}/src/mesa/main/varray.c"
  "${MESA}/src/mesa/main/vtxfmt.c"
  "${MESA}/src/mesa/math/m_debug_clip.c"
  "${MESA}/src/mesa/math/m_debug_norm.c"
  "${MESA}/src/mesa/math/m_debug_xform.c"
  "${MESA}/src/mesa/math/m_eval.c"
  "${MESA}/src/mesa/math/m_matrix.c"
  "${MESA}/src/mesa/math/m_translate.c"
  "${MESA}/src/mesa/math/m_vector.c"
  "${MESA}/src/mesa/math/m_xform.c"
  "${MESA}/src/mesa/shader/arbprogparse.c"
  "${MESA}/src/mesa/shader/arbprogram.c"
  "${MESA}/src/mesa/shader/atifragshader.c"
  "${MESA}/src/mesa/shader/grammar/grammar_mesa.c"
  "${MESA}/src/mesa/shader/nvfragparse.c"
  "${MESA}/src/mesa/shader/nvprogram.c"
  "${MESA}/src/mesa/shader/nvvertexec.c"
  "${MESA}/src/mesa/shader/nvvertparse.c"
  "${MESA}/src/mesa/shader/program.c"
  "${MESA}/src/mesa/shader/shaderobjects.c"
  "${MESA}/src/mesa/shader/shaderobjects_3dlabs.c"
  "${MESA}/src/mesa/shader/slang/slang_analyse.c"
  "${MESA}/src/mesa/shader/slang/slang_assemble.c"
  "${MESA}/src/mesa/shader/slang/slang_assemble_assignment.c"
  "${MESA}/src/mesa/shader/slang/slang_assemble_conditional.c"
  "${MESA}/src/mesa/shader/slang/slang_assemble_constructor.c"
  "${MESA}/src/mesa/shader/slang/slang_assemble_typeinfo.c"
  "${MESA}/src/mesa/shader/slang/slang_compile.c"
  "${MESA}/src/mesa/shader/slang/slang_compile_function.c"
  "${MESA}/src/mesa/shader/slang/slang_compile_operation.c"
  "${MESA}/src/mesa/shader/slang/slang_compile_struct.c"
  "${MESA}/src/mesa/shader/slang/slang_compile_variable.c"
  "${MESA}/src/mesa/shader/slang/slang_execute.c"
  "${MESA}/src/mesa/shader/slang/slang_execute_x86.c"
  "${MESA}/src/mesa/shader/slang/slang_export.c"
  "${MESA}/src/mesa/shader/slang/slang_library_noise.c"
  "${MESA}/src/mesa/shader/slang/slang_library_texsample.c"
  "${MESA}/src/mesa/shader/slang/slang_link.c"
  "${MESA}/src/mesa/shader/slang/slang_preprocess.c"
  "${MESA}/src/mesa/shader/slang/slang_storage.c"
  "${MESA}/src/mesa/shader/slang/slang_utility.c"
  "${MESA}/src/mesa/swrast/s_aaline.c"
  "${MESA}/src/mesa/swrast/s_aatriangle.c"
  "${MESA}/src/mesa/swrast/s_accum.c"
  "${MESA}/src/mesa/swrast/s_alpha.c"
  "${MESA}/src/mesa/swrast/s_arbshader.c"
  "${MESA}/src/mesa/swrast/s_atifragshader.c"
  "${MESA}/src/mesa/swrast/s_bitmap.c"
  "${MESA}/src/mesa/swrast/s_blend.c"
  "${MESA}/src/mesa/swrast/s_blit.c"
  "${MESA}/src/mesa/swrast/s_buffers.c"
  "${MESA}/src/mesa/swrast/s_context.c"
  "${MESA}/src/mesa/swrast/s_copypix.c"
  "${MESA}/src/mesa/swrast/s_depth.c"
  "${MESA}/src/mesa/swrast/s_drawpix.c"
  "${MESA}/src/mesa/swrast/s_feedback.c"
  "${MESA}/src/mesa/swrast/s_fog.c"
  "${MESA}/src/mesa/swrast/s_imaging.c"
  "${MESA}/src/mesa/swrast/s_lines.c"
  "${MESA}/src/mesa/swrast/s_logic.c"
  "${MESA}/src/mesa/swrast/s_masking.c"
  "${MESA}/src/mesa/swrast/s_nvfragprog.c"
  "${MESA}/src/mesa/swrast/s_points.c"
  "${MESA}/src/mesa/swrast/s_readpix.c"
  "${MESA}/src/mesa/swrast/s_span.c"
  "${MESA}/src/mesa/swrast/s_stencil.c"
  "${MESA}/src/mesa/swrast/s_texcombine.c"
  "${MESA}/src/mesa/swrast/s_texfilter.c"
  "${MESA}/src/mesa/swrast/s_texstore.c"
  "${MESA}/src/mesa/swrast/s_triangle.c"
  "${MESA}/src/mesa/swrast/s_zoom.c"
  "${MESA}/src/mesa/swrast_setup/ss_context.c"
  "${MESA}/src/mesa/swrast_setup/ss_triangle.c"
  "${MESA}/src/mesa/tnl/t_array_api.c"
  "${MESA}/src/mesa/tnl/t_array_import.c"
  "${MESA}/src/mesa/tnl/t_context.c"
  "${MESA}/src/mesa/tnl/t_pipeline.c"
  "${MESA}/src/mesa/tnl/t_save_api.c"
  "${MESA}/src/mesa/tnl/t_save_loopback.c"
  "${MESA}/src/mesa/tnl/t_save_playback.c"
  "${MESA}/src/mesa/tnl/t_vb_arbprogram.c"
  "${MESA}/src/mesa/tnl/t_vb_arbshader.c"
  "${MESA}/src/mesa/tnl/t_vb_cull.c"
  "${MESA}/src/mesa/tnl/t_vb_fog.c"
  "${MESA}/src/mesa/tnl/t_vb_light.c"
  "${MESA}/src/mesa/tnl/t_vb_normals.c"
  "${MESA}/src/mesa/tnl/t_vb_points.c"
  "${MESA}/src/mesa/tnl/t_vb_program.c"
  "${MESA}/src/mesa/tnl/t_vb_render.c"
  "${MESA}/src/mesa/tnl/t_vb_texgen.c"
  "${MESA}/src/mesa/tnl/t_vb_texmat.c"
  "${MESA}/src/mesa/tnl/t_vb_vertex.c"
  "${MESA}/src/mesa/tnl/t_vertex.c"
  "${MESA}/src/mesa/tnl/t_vertex_generic.c"
  "${MESA}/src/mesa/tnl/t_vp_build.c"
  "${MESA}/src/mesa/tnl/t_vtx_api.c"
  "${MESA}/src/mesa/tnl/t_vtx_eval.c"
  "${MESA}/src/mesa/tnl/t_vtx_exec.c"
  "${MESA}/src/mesa/tnl/t_vtx_generic.c"
)

set(OPENRBX_MESA_GDI_SOURCES
  "${MESA}/src/mesa/drivers/common/driverfuncs.c"
  "${MESA}/src/mesa/drivers/windows/gdi/wgl.c"
  "${MESA}/src/mesa/drivers/windows/gdi/wmesa.c"
)

set(OPENRBX_MESA_OSMESA_SOURCES
  "${MESA}/src/mesa/drivers/common/driverfuncs.c"
  "${MESA}/src/mesa/drivers/osmesa/osmesa.c"
)

set(OPENRBX_MESA_GLU_SOURCES
  "${MESA}/src/glu/sgi/libnurbs/interface/bezierEval.cc"
  "${MESA}/src/glu/sgi/libnurbs/interface/bezierPatch.cc"
  "${MESA}/src/glu/sgi/libnurbs/interface/bezierPatchMesh.cc"
  "${MESA}/src/glu/sgi/libnurbs/interface/glcurveval.cc"
  "${MESA}/src/glu/sgi/libnurbs/interface/glinterface.cc"
  "${MESA}/src/glu/sgi/libnurbs/interface/glrenderer.cc"
  "${MESA}/src/glu/sgi/libnurbs/interface/glsurfeval.cc"
  "${MESA}/src/glu/sgi/libnurbs/interface/incurveeval.cc"
  "${MESA}/src/glu/sgi/libnurbs/interface/insurfeval.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/arc.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/arcsorter.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/arctess.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/backend.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/basiccrveval.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/basicsurfeval.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/bin.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/bufpool.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/cachingeval.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/ccw.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/coveandtiler.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/curve.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/curvelist.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/curvesub.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/dataTransform.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/displaylist.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/flist.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/flistsorter.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/hull.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/intersect.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/knotvector.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/mapdesc.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/mapdescv.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/maplist.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/mesher.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/monoTriangulationBackend.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/monotonizer.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/mycode.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/nurbsinterfac.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/nurbstess.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/patch.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/patchlist.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/quilt.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/reader.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/renderhints.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/slicer.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/sorter.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/splitarcs.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/subdivider.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/tobezier.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/trimline.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/trimregion.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/trimvertpool.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/uarray.cc"
  "${MESA}/src/glu/sgi/libnurbs/internals/varray.cc"
  "${MESA}/src/glu/sgi/libnurbs/nurbtess/directedLine.cc"
  "${MESA}/src/glu/sgi/libnurbs/nurbtess/gridWrap.cc"
  "${MESA}/src/glu/sgi/libnurbs/nurbtess/monoChain.cc"
  "${MESA}/src/glu/sgi/libnurbs/nurbtess/monoPolyPart.cc"
  "${MESA}/src/glu/sgi/libnurbs/nurbtess/monoTriangulation.cc"
  "${MESA}/src/glu/sgi/libnurbs/nurbtess/partitionX.cc"
  "${MESA}/src/glu/sgi/libnurbs/nurbtess/partitionY.cc"
  "${MESA}/src/glu/sgi/libnurbs/nurbtess/polyDBG.cc"
  "${MESA}/src/glu/sgi/libnurbs/nurbtess/polyUtil.cc"
  "${MESA}/src/glu/sgi/libnurbs/nurbtess/primitiveStream.cc"
  "${MESA}/src/glu/sgi/libnurbs/nurbtess/quicksort.cc"
  "${MESA}/src/glu/sgi/libnurbs/nurbtess/rectBlock.cc"
  "${MESA}/src/glu/sgi/libnurbs/nurbtess/sampleComp.cc"
  "${MESA}/src/glu/sgi/libnurbs/nurbtess/sampleCompBot.cc"
  "${MESA}/src/glu/sgi/libnurbs/nurbtess/sampleCompRight.cc"
  "${MESA}/src/glu/sgi/libnurbs/nurbtess/sampleCompTop.cc"
  "${MESA}/src/glu/sgi/libnurbs/nurbtess/sampleMonoPoly.cc"
  "${MESA}/src/glu/sgi/libnurbs/nurbtess/sampledLine.cc"
  "${MESA}/src/glu/sgi/libnurbs/nurbtess/searchTree.cc"
  "${MESA}/src/glu/sgi/libtess/dict.c"
  "${MESA}/src/glu/sgi/libtess/geom.c"
  "${MESA}/src/glu/sgi/libtess/memalloc.c"
  "${MESA}/src/glu/sgi/libtess/mesh.c"
  "${MESA}/src/glu/sgi/libtess/normal.c"
  "${MESA}/src/glu/sgi/libtess/priorityq.c"
  "${MESA}/src/glu/sgi/libtess/render.c"
  "${MESA}/src/glu/sgi/libtess/sweep.c"
  "${MESA}/src/glu/sgi/libtess/tess.c"
  "${MESA}/src/glu/sgi/libtess/tessmono.c"
  "${MESA}/src/glu/sgi/libutil/error.c"
  "${MESA}/src/glu/sgi/libutil/glue.c"
  "${MESA}/src/glu/sgi/libutil/mipmap.c"
  "${MESA}/src/glu/sgi/libutil/project.c"
  "${MESA}/src/glu/sgi/libutil/quad.c"
  "${MESA}/src/glu/sgi/libutil/registry.c"
)

set(OPENRBX_MESA_LINK_OPTIONS "/INCREMENTAL:NO")

set(OPENRBX_MESA_COMPILE_OPTIONS "/O2" "/Ob1" "/GF" "/Gy" "/Zm1000" "/W0")

add_library(mesa_core STATIC ${OPENRBX_MESA_CORE_SOURCES})
set_target_properties(mesa_core PROPERTIES MSVC_RUNTIME_LIBRARY "MultiThreaded")
target_include_directories(mesa_core PRIVATE
  "${MESA}/include" "${MESA}/src/mesa" "${MESA}/src/mesa/glapi" "${MESA}/src/mesa/main"
  "${MESA}/src/mesa/shader" "${MESA}/src/mesa/shader/slang" "${MESA}/src/mesa/shader/grammar"
)
target_compile_definitions(mesa_core PRIVATE NDEBUG WIN32 _MBCS _LIB _DLL BUILD_GL32 MESA_MINWARN)
target_compile_options(mesa_core PRIVATE ${OPENRBX_MESA_COMPILE_OPTIONS})

add_library(opengl32 SHARED ${OPENRBX_MESA_GDI_SOURCES})
set_target_properties(opengl32 PROPERTIES OUTPUT_NAME "OPENGL32" SUFFIX ".DLL"
  MSVC_RUNTIME_LIBRARY "MultiThreaded")
target_include_directories(opengl32 PRIVATE
  "${MESA}/windows/VC7/main" "${MESA}/include" "${MESA}/src/mesa" "${MESA}/src/mesa/main"
  "${MESA}/src/mesa/glapi" "${MESA}/src/mesa/swrast" "${MESA}/src/mesa/shader"
)
target_compile_definitions(opengl32 PRIVATE
  NDEBUG WIN32 _WINDOWS _MBCS _USRDLL GDI_EXPORTS _DLL BUILD_GL32 MESA_MINWARN)
target_compile_options(opengl32 PRIVATE ${OPENRBX_MESA_COMPILE_OPTIONS})
target_link_libraries(opengl32 PRIVATE mesa_core winmm msvcrt kernel32 user32 gdi32 advapi32)
target_link_options(opengl32 PRIVATE ${OPENRBX_MESA_LINK_OPTIONS}
  "/DEF:${MESA}/src/mesa/drivers/windows/gdi/mesa.def")

add_library(osmesa32 SHARED ${OPENRBX_MESA_OSMESA_SOURCES})
set_target_properties(osmesa32 PROPERTIES OUTPUT_NAME "OSMESA32" SUFFIX ".DLL"
  MSVC_RUNTIME_LIBRARY "MultiThreaded")
target_include_directories(osmesa32 PRIVATE
  "${MESA}/include" "${MESA}/src/mesa" "${MESA}/src/mesa/main" "${MESA}/src/mesa/glapi"
  "${MESA}/src/mesa/swrast" "${MESA}/src/mesa/shader"
)
target_compile_definitions(osmesa32 PRIVATE
  WIN32 NDEBUG _WINDOWS _MBCS _USRDLL OSMESA_EXPORTS)
target_compile_options(osmesa32 PRIVATE ${OPENRBX_MESA_COMPILE_OPTIONS})
target_link_libraries(osmesa32 PRIVATE opengl32 winmm msvcrt kernel32 user32 gdi32 advapi32)
target_link_options(osmesa32 PRIVATE ${OPENRBX_MESA_LINK_OPTIONS}
  "/DEF:${MESA}/src/mesa/drivers/osmesa/osmesa.def")

add_library(glu32 SHARED ${OPENRBX_MESA_GLU_SOURCES})
set_target_properties(glu32 PROPERTIES OUTPUT_NAME "GLU32" SUFFIX ".DLL"
  MSVC_RUNTIME_LIBRARY "MultiThreaded")
target_include_directories(glu32 PRIVATE
  "${MESA}/include" "${MESA}/src/glu/sgi/include" "${MESA}/src/glu/sgi/libnurbs/interface"
  "${MESA}/src/glu/sgi/libnurbs/internals" "${MESA}/src/glu/sgi/libnurbs/nurbtess"
)
target_compile_definitions(glu32 PRIVATE
  WIN32 NDEBUG _WINDOWS _MBCS _USRDLL GLU_EXPORTS BUILD_GL32)
target_compile_options(glu32 PRIVATE ${OPENRBX_MESA_COMPILE_OPTIONS})

set(OPENRBX_MESA_GLU_NURBS_SOURCES ${OPENRBX_MESA_GLU_SOURCES})
list(FILTER OPENRBX_MESA_GLU_NURBS_SOURCES INCLUDE REGEX "[.]cc$")
set_source_files_properties(${OPENRBX_MESA_GLU_NURBS_SOURCES} DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}"
  PROPERTIES COMPILE_OPTIONS "/EHs-c-" COMPILE_DEFINITIONS "LIBRARYBUILD")
target_link_libraries(glu32 PRIVATE msvcrt winmm opengl32 kernel32 user32 gdi32 advapi32)
target_link_options(glu32 PRIVATE ${OPENRBX_MESA_LINK_OPTIONS}
  "/DEF:${MESA}/src/glu/sgi/glu.def")

set(SCINTILLA "${PROJECT_SOURCE_DIR}/3rdparty/scintilla")
set(OGRE "${PROJECT_SOURCE_DIR}/3rdparty/ogrenew")
set(DXSDK "${PROJECT_SOURCE_DIR}/3rdparty/Microsoft DirectX 9.0 SDK (October 2004)")

set(OPENRBX_SCILEXER_SOURCES
  "${SCINTILLA}/src/AutoComplete.cxx"
  "${SCINTILLA}/src/CallTip.cxx"
  "${SCINTILLA}/src/CellBuffer.cxx"
  "${SCINTILLA}/src/CharClassify.cxx"
  "${SCINTILLA}/src/ContractionState.cxx"
  "${SCINTILLA}/src/Document.cxx"
  "${SCINTILLA}/src/DocumentAccessor.cxx"
  "${SCINTILLA}/src/Editor.cxx"
  "${SCINTILLA}/src/ExternalLexer.cxx"
  "${SCINTILLA}/src/Indicator.cxx"
  "${SCINTILLA}/src/KeyMap.cxx"
  "${SCINTILLA}/src/KeyWords.cxx"
  "${SCINTILLA}/src/LexAda.cxx"
  "${SCINTILLA}/src/LexAPDL.cxx"
  "${SCINTILLA}/src/LexAsm.cxx"
  "${SCINTILLA}/src/LexAsn1.cxx"
  "${SCINTILLA}/src/LexAU3.cxx"
  "${SCINTILLA}/src/LexAVE.cxx"
  "${SCINTILLA}/src/LexBaan.cxx"
  "${SCINTILLA}/src/LexBash.cxx"
  "${SCINTILLA}/src/LexBasic.cxx"
  "${SCINTILLA}/src/LexBullant.cxx"
  "${SCINTILLA}/src/LexCaml.cxx"
  "${SCINTILLA}/src/LexCLW.cxx"
  "${SCINTILLA}/src/LexConf.cxx"
  "${SCINTILLA}/src/LexCPP.cxx"
  "${SCINTILLA}/src/LexCrontab.cxx"
  "${SCINTILLA}/src/LexCsound.cxx"
  "${SCINTILLA}/src/LexCSS.cxx"
  "${SCINTILLA}/src/LexEiffel.cxx"
  "${SCINTILLA}/src/LexErlang.cxx"
  "${SCINTILLA}/src/LexEScript.cxx"
  "${SCINTILLA}/src/LexFlagship.cxx"
  "${SCINTILLA}/src/LexForth.cxx"
  "${SCINTILLA}/src/LexFortran.cxx"
  "${SCINTILLA}/src/LexGui4Cli.cxx"
  "${SCINTILLA}/src/LexHaskell.cxx"
  "${SCINTILLA}/src/LexHTML.cxx"
  "${SCINTILLA}/src/LexInno.cxx"
  "${SCINTILLA}/src/LexKix.cxx"
  "${SCINTILLA}/src/LexLisp.cxx"
  "${SCINTILLA}/src/LexLout.cxx"
  "${SCINTILLA}/src/LexLua.cxx"
  "${SCINTILLA}/src/LexMatlab.cxx"
  "${SCINTILLA}/src/LexMetapost.cxx"
  "${SCINTILLA}/src/LexMMIXAL.cxx"
  "${SCINTILLA}/src/LexMPT.cxx"
  "${SCINTILLA}/src/LexMSSQL.cxx"
  "${SCINTILLA}/src/LexNsis.cxx"
  "${SCINTILLA}/src/LexOpal.cxx"
  "${SCINTILLA}/src/LexOthers.cxx"
  "${SCINTILLA}/src/LexPascal.cxx"
  "${SCINTILLA}/src/LexPB.cxx"
  "${SCINTILLA}/src/LexPerl.cxx"
  "${SCINTILLA}/src/LexPOV.cxx"
  "${SCINTILLA}/src/LexPS.cxx"
  "${SCINTILLA}/src/LexPython.cxx"
  "${SCINTILLA}/src/LexRebol.cxx"
  "${SCINTILLA}/src/LexRuby.cxx"
  "${SCINTILLA}/src/LexScriptol.cxx"
  "${SCINTILLA}/src/LexSmalltalk.cxx"
  "${SCINTILLA}/src/LexSpecman.cxx"
  "${SCINTILLA}/src/LexSpice.cxx"
  "${SCINTILLA}/src/LexSQL.cxx"
  "${SCINTILLA}/src/LexTADS3.cxx"
  "${SCINTILLA}/src/LexTCL.cxx"
  "${SCINTILLA}/src/LexTeX.cxx"
  "${SCINTILLA}/src/LexVB.cxx"
  "${SCINTILLA}/src/LexVerilog.cxx"
  "${SCINTILLA}/src/LexVHDL.cxx"
  "${SCINTILLA}/src/LexYAML.cxx"
  "${SCINTILLA}/src/LineMarker.cxx"
  "${SCINTILLA}/win32/PlatWin.cxx"
  "${SCINTILLA}/src/PropSet.cxx"
  "${SCINTILLA}/src/RESearch.cxx"
  "${SCINTILLA}/src/ScintillaBase.cxx"
  "${SCINTILLA}/win32/ScintillaWin.cxx"
  "${SCINTILLA}/src/Style.cxx"
  "${SCINTILLA}/src/StyleContext.cxx"
  "${SCINTILLA}/src/UniConversion.cxx"
  "${SCINTILLA}/src/ViewStyle.cxx"
  "${SCINTILLA}/src/WindowAccessor.cxx"
  "${SCINTILLA}/src/XPM.cxx"
  "${SCINTILLA}/win32/ScintRes.rc"
)

set(OPENRBX_OGREMAIN_SOURCES
  "${OGRE}/OgreMain/src/OgreAlignedAllocator.cpp"
  "${OGRE}/OgreMain/src/OgreAnimable.cpp"
  "${OGRE}/OgreMain/src/OgreAnimation.cpp"
  "${OGRE}/OgreMain/src/OgreAnimationState.cpp"
  "${OGRE}/OgreMain/src/OgreAnimationTrack.cpp"
  "${OGRE}/OgreMain/src/OgreArchiveManager.cpp"
  "${OGRE}/OgreMain/src/OgreAutoParamDataSource.cpp"
  "${OGRE}/OgreMain/src/OgreBillboard.cpp"
  "${OGRE}/OgreMain/src/OgreBillboardChain.cpp"
  "${OGRE}/OgreMain/src/OgreBillboardParticleRenderer.cpp"
  "${OGRE}/OgreMain/src/OgreBillboardSet.cpp"
  "${OGRE}/OgreMain/src/OgreBone.cpp"
  "${OGRE}/OgreMain/src/OgreBorderPanelOverlayElement.cpp"
  "${OGRE}/OgreMain/src/OgreCamera.cpp"
  "${OGRE}/OgreMain/src/OgreCodec.cpp"
  "${OGRE}/OgreMain/src/OgreColourValue.cpp"
  "${OGRE}/OgreMain/src/OgreCommon.cpp"
  "${OGRE}/OgreMain/src/OgreCompiler2Pass.cpp"
  "${OGRE}/OgreMain/src/OgreCompositionPass.cpp"
  "${OGRE}/OgreMain/src/OgreCompositionTargetPass.cpp"
  "${OGRE}/OgreMain/src/OgreCompositionTechnique.cpp"
  "${OGRE}/OgreMain/src/OgreCompositor.cpp"
  "${OGRE}/OgreMain/src/OgreCompositorChain.cpp"
  "${OGRE}/OgreMain/src/OgreCompositorInstance.cpp"
  "${OGRE}/OgreMain/src/OgreCompositorManager.cpp"
  "${OGRE}/OgreMain/src/OgreCompositorScriptCompiler.cpp"
  "${OGRE}/OgreMain/src/OgreCompositorSerializer.cpp"
  "${OGRE}/OgreMain/src/WIN32/OgreConfigDialog.cpp"
  "${OGRE}/OgreMain/src/OgreConfigFile.cpp"
  "${OGRE}/OgreMain/src/OgreControllerManager.cpp"
  "${OGRE}/OgreMain/src/OgreConvexBody.cpp"
  "${OGRE}/OgreMain/src/OgreDataStream.cpp"
  "${OGRE}/OgreMain/src/OgreDDSCodec.cpp"
  "${OGRE}/OgreMain/src/OgreDefaultHardwareBufferManager.cpp"
  "${OGRE}/OgreMain/src/OgreDefaultSceneQueries.cpp"
  "${OGRE}/OgreMain/src/OgreDynLib.cpp"
  "${OGRE}/OgreMain/src/OgreDynLibManager.cpp"
  "${OGRE}/OgreMain/src/OgreEdgeListBuilder.cpp"
  "${OGRE}/OgreMain/src/OgreEntity.cpp"
  "${OGRE}/OgreMain/src/WIN32/OgreErrorDialog.cpp"
  "${OGRE}/OgreMain/src/OgreException.cpp"
  "${OGRE}/OgreMain/src/OgreExternalTextureSource.cpp"
  "${OGRE}/OgreMain/src/OgreExternalTextureSourceManager.cpp"
  "${OGRE}/OgreMain/src/OgreFileSystem.cpp"
  "${OGRE}/OgreMain/src/OgreFont.cpp"
  "${OGRE}/OgreMain/src/OgreFontManager.cpp"
  "${OGRE}/OgreMain/src/OgreFreeImageCodec.cpp"
  "${OGRE}/OgreMain/src/OgreFrustum.cpp"
  "${OGRE}/OgreMain/src/OgreGpuProgram.cpp"
  "${OGRE}/OgreMain/src/OgreGpuProgramManager.cpp"
  "${OGRE}/OgreMain/src/OgreGpuProgramUsage.cpp"
  "${OGRE}/OgreMain/src/OgreHardwareBufferManager.cpp"
  "${OGRE}/OgreMain/src/OgreHardwareIndexBuffer.cpp"
  "${OGRE}/OgreMain/src/OgreHardwareOcclusionQuery.cpp"
  "${OGRE}/OgreMain/src/OgreHardwarePixelBuffer.cpp"
  "${OGRE}/OgreMain/src/OgreHardwareVertexBuffer.cpp"
  "${OGRE}/OgreMain/src/OgreHighLevelGpuProgram.cpp"
  "${OGRE}/OgreMain/src/OgreHighLevelGpuProgramManager.cpp"
  "${OGRE}/OgreMain/src/OgreImage.cpp"
  "${OGRE}/OgreMain/src/OgreInstancedGeometry.cpp"
  "${OGRE}/OgreMain/src/OgreKeyFrame.cpp"
  "${OGRE}/OgreMain/src/OgreLight.cpp"
  "${OGRE}/OgreMain/src/OgreLog.cpp"
  "${OGRE}/OgreMain/src/OgreLogManager.cpp"
  "${OGRE}/OgreMain/src/OgreManualObject.cpp"
  "${OGRE}/OgreMain/src/OgreMaterial.cpp"
  "${OGRE}/OgreMain/src/OgreMaterialManager.cpp"
  "${OGRE}/OgreMain/src/OgreMaterialScriptCompiler.cpp"
  "${OGRE}/OgreMain/src/OgreMaterialSerializer.cpp"
  "${OGRE}/OgreMain/src/OgreMath.cpp"
  "${OGRE}/OgreMain/src/OgreMatrix3.cpp"
  "${OGRE}/OgreMain/src/OgreMatrix4.cpp"
  "${OGRE}/OgreMain/src/OgreMemoryManager.cpp"
  "${OGRE}/OgreMain/src/OgreMesh.cpp"
  "${OGRE}/OgreMain/src/OgreMeshManager.cpp"
  "${OGRE}/OgreMain/src/OgreMeshSerializer.cpp"
  "${OGRE}/OgreMain/src/OgreMeshSerializerImpl.cpp"
  "${OGRE}/OgreMain/src/OgreMovableObject.cpp"
  "${OGRE}/OgreMain/src/OgreMovablePlane.cpp"
  "${OGRE}/OgreMain/src/OgreNode.cpp"
  "${OGRE}/OgreMain/src/OgreNumerics.cpp"
  "${OGRE}/OgreMain/src/OgreOptimisedUtil.cpp"
  "${OGRE}/OgreMain/src/OgreOptimisedUtilGeneral.cpp"
  "${OGRE}/OgreMain/src/OgreOptimisedUtilSSE.cpp"
  "${OGRE}/OgreMain/src/OgreOverlay.cpp"
  "${OGRE}/OgreMain/src/OgreOverlayContainer.cpp"
  "${OGRE}/OgreMain/src/OgreOverlayElement.cpp"
  "${OGRE}/OgreMain/src/OgreOverlayElementCommands.cpp"
  "${OGRE}/OgreMain/src/OgreOverlayManager.cpp"
  "${OGRE}/OgreMain/src/OgrePanelOverlayElement.cpp"
  "${OGRE}/OgreMain/src/OgreParticle.cpp"
  "${OGRE}/OgreMain/src/OgreParticleEmitter.cpp"
  "${OGRE}/OgreMain/src/OgreParticleEmitterCommands.cpp"
  "${OGRE}/OgreMain/src/OgreParticleIterator.cpp"
  "${OGRE}/OgreMain/src/OgreParticleSystem.cpp"
  "${OGRE}/OgreMain/src/OgreParticleSystemManager.cpp"
  "${OGRE}/OgreMain/src/OgrePass.cpp"
  "${OGRE}/OgreMain/src/OgrePatchMesh.cpp"
  "${OGRE}/OgreMain/src/OgrePatchSurface.cpp"
  "${OGRE}/OgreMain/src/OgrePixelFormat.cpp"
  "${OGRE}/OgreMain/src/OgrePlane.cpp"
  "${OGRE}/OgreMain/src/OgrePlatformInformation.cpp"
  "${OGRE}/OgreMain/src/OgrePolygon.cpp"
  "${OGRE}/OgreMain/src/OgrePose.cpp"
  "${OGRE}/OgreMain/src/OgrePrecompiledHeaders.cpp"
  "${OGRE}/OgreMain/src/OgrePredefinedControllers.cpp"
  "${OGRE}/OgreMain/src/OgrePrefabFactory.cpp"
  "${OGRE}/OgreMain/src/OgreProfiler.cpp"
  "${OGRE}/OgreMain/src/OgreProgressiveMesh.cpp"
  "${OGRE}/OgreMain/src/OgreQuaternion.cpp"
  "${OGRE}/OgreMain/src/OgreRectangle2D.cpp"
  "${OGRE}/OgreMain/src/OgreRenderQueue.cpp"
  "${OGRE}/OgreMain/src/OgreRenderQueueInvocation.cpp"
  "${OGRE}/OgreMain/src/OgreRenderQueueSortingGrouping.cpp"
  "${OGRE}/OgreMain/src/OgreRenderSystem.cpp"
  "${OGRE}/OgreMain/src/OgreRenderSystemCapabilities.cpp"
  "${OGRE}/OgreMain/src/OgreRenderTarget.cpp"
  "${OGRE}/OgreMain/src/OgreRenderTexture.cpp"
  "${OGRE}/OgreMain/src/OgreRenderWindow.cpp"
  "${OGRE}/OgreMain/src/OgreResource.cpp"
  "${OGRE}/OgreMain/src/OgreResourceBackgroundQueue.cpp"
  "${OGRE}/OgreMain/src/OgreResourceGroupManager.cpp"
  "${OGRE}/OgreMain/src/OgreResourceManager.cpp"
  "${OGRE}/OgreMain/src/OgreRibbonTrail.cpp"
  "${OGRE}/OgreMain/src/OgreRoot.cpp"
  "${OGRE}/OgreMain/src/OgreRotationSpline.cpp"
  "${OGRE}/OgreMain/src/OgreSceneManager.cpp"
  "${OGRE}/OgreMain/src/OgreSceneManagerEnumerator.cpp"
  "${OGRE}/OgreMain/src/OgreSceneNode.cpp"
  "${OGRE}/OgreMain/src/OgreSceneQuery.cpp"
  "${OGRE}/OgreMain/src/OgreSerializer.cpp"
  "${OGRE}/OgreMain/src/OgreShadowCameraSetup.cpp"
  "${OGRE}/OgreMain/src/OgreShadowCameraSetupFocused.cpp"
  "${OGRE}/OgreMain/src/OgreShadowCameraSetupLiSPSM.cpp"
  "${OGRE}/OgreMain/src/OgreShadowCameraSetupPlaneOptimal.cpp"
  "${OGRE}/OgreMain/src/OgreShadowCaster.cpp"
  "${OGRE}/OgreMain/src/OgreShadowTextureManager.cpp"
  "${OGRE}/OgreMain/src/OgreShadowVolumeExtrudeProgram.cpp"
  "${OGRE}/OgreMain/src/OgreSimpleRenderable.cpp"
  "${OGRE}/OgreMain/src/OgreSimpleSpline.cpp"
  "${OGRE}/OgreMain/src/OgreSkeleton.cpp"
  "${OGRE}/OgreMain/src/OgreSkeletonInstance.cpp"
  "${OGRE}/OgreMain/src/OgreSkeletonManager.cpp"
  "${OGRE}/OgreMain/src/OgreSkeletonSerializer.cpp"
  "${OGRE}/OgreMain/src/OgreStaticGeometry.cpp"
  "${OGRE}/OgreMain/src/OgreString.cpp"
  "${OGRE}/OgreMain/src/OgreStringConverter.cpp"
  "${OGRE}/OgreMain/src/OgreStringInterface.cpp"
  "${OGRE}/OgreMain/src/OgreSubEntity.cpp"
  "${OGRE}/OgreMain/src/OgreSubMesh.cpp"
  "${OGRE}/OgreMain/src/OgreTagPoint.cpp"
  "${OGRE}/OgreMain/src/OgreTechnique.cpp"
  "${OGRE}/OgreMain/src/OgreTextAreaOverlayElement.cpp"
  "${OGRE}/OgreMain/src/OgreTexture.cpp"
  "${OGRE}/OgreMain/src/OgreTextureManager.cpp"
  "${OGRE}/OgreMain/src/OgreTextureUnitState.cpp"
  "${OGRE}/OgreMain/src/WIN32/OgreTimer.cpp"
  "${OGRE}/OgreMain/src/OgreUnifiedHighLevelGpuProgram.cpp"
  "${OGRE}/OgreMain/src/OgreUserDefinedObject.cpp"
  "${OGRE}/OgreMain/src/OgreVector2.cpp"
  "${OGRE}/OgreMain/src/OgreVector3.cpp"
  "${OGRE}/OgreMain/src/OgreVector4.cpp"
  "${OGRE}/OgreMain/src/OgreVertexIndexData.cpp"
  "${OGRE}/OgreMain/src/OgreViewport.cpp"
  "${OGRE}/OgreMain/src/OgreWindowEventUtilities.cpp"
  "${OGRE}/OgreMain/src/OgreWireBoundingBox.cpp"
  "${OGRE}/OgreMain/src/OgreZip.cpp"
  "${OGRE}/OgreMain/src/WIN32/OgreWin32Resources.rc"
)

set(OPENRBX_PARTICLEFX_SOURCES
  "${OGRE}/PlugIns/ParticleFX/src/OgreAreaEmitter.cpp"
  "${OGRE}/PlugIns/ParticleFX/src/OgreBoxEmitter.cpp"
  "${OGRE}/PlugIns/ParticleFX/src/OgreColourFaderAffector.cpp"
  "${OGRE}/PlugIns/ParticleFX/src/OgreColourFaderAffector2.cpp"
  "${OGRE}/PlugIns/ParticleFX/src/OgreColourImageAffector.cpp"
  "${OGRE}/PlugIns/ParticleFX/src/OgreColourInterpolatorAffector.cpp"
  "${OGRE}/PlugIns/ParticleFX/src/OgreCylinderEmitter.cpp"
  "${OGRE}/PlugIns/ParticleFX/src/OgreDeflectorPlaneAffector.cpp"
  "${OGRE}/PlugIns/ParticleFX/src/OgreDirectionRandomiserAffector.cpp"
  "${OGRE}/PlugIns/ParticleFX/src/OgreEllipsoidEmitter.cpp"
  "${OGRE}/PlugIns/ParticleFX/src/OgreHollowEllipsoidEmitter.cpp"
  "${OGRE}/PlugIns/ParticleFX/src/OgreLinearForceAffector.cpp"
  "${OGRE}/PlugIns/ParticleFX/src/OgreParticleFX.cpp"
  "${OGRE}/PlugIns/ParticleFX/src/OgreParticleFXPlugin.cpp"
  "${OGRE}/PlugIns/ParticleFX/src/OgrePointEmitter.cpp"
  "${OGRE}/PlugIns/ParticleFX/src/OgreRingEmitter.cpp"
  "${OGRE}/PlugIns/ParticleFX/src/OgreRotationAffector.cpp"
  "${OGRE}/PlugIns/ParticleFX/src/OgreScaleAffector.cpp"
)

set(OPENRBX_GL_SOURCES
  "${OGRE}/RenderSystems/GL/src/glew.cpp"
  "${OGRE}/RenderSystems/GL/src/OgreGLATIFSInit.cpp"
  "${OGRE}/RenderSystems/GL/src/OgreGLContext.cpp"
  "${OGRE}/RenderSystems/GL/src/OgreGLDefaultHardwareBufferManager.cpp"
  "${OGRE}/RenderSystems/GL/src/OgreGLEngineDll.cpp"
  "${OGRE}/RenderSystems/GL/src/OgreGLFBOMultiRenderTarget.cpp"
  "${OGRE}/RenderSystems/GL/src/OgreGLFBORenderTexture.cpp"
  "${OGRE}/RenderSystems/GL/src/OgreGLFrameBufferObject.cpp"
  "${OGRE}/RenderSystems/GL/src/OgreGLGpuNvparseProgram.cpp"
  "${OGRE}/RenderSystems/GL/src/OgreGLGpuProgram.cpp"
  "${OGRE}/RenderSystems/GL/src/OgreGLGpuProgramManager.cpp"
  "${OGRE}/RenderSystems/GL/src/OgreGLHardwareBufferManager.cpp"
  "${OGRE}/RenderSystems/GL/src/OgreGLHardwareIndexBuffer.cpp"
  "${OGRE}/RenderSystems/GL/src/OgreGLHardwareOcclusionQuery.cpp"
  "${OGRE}/RenderSystems/GL/src/OgreGLHardwarePixelBuffer.cpp"
  "${OGRE}/RenderSystems/GL/src/OgreGLHardwareVertexBuffer.cpp"
  "${OGRE}/RenderSystems/GL/src/OgreGLPBRenderTexture.cpp"
  "${OGRE}/RenderSystems/GL/src/OgreGLPixelFormat.cpp"
  "${OGRE}/RenderSystems/GL/src/OgreGLPlugin.cpp"
  "${OGRE}/RenderSystems/GL/src/OgreGLRenderSystem.cpp"
  "${OGRE}/RenderSystems/GL/src/OgreGLRenderTexture.cpp"
  "${OGRE}/RenderSystems/GL/src/OgreGLSupport.cpp"
  "${OGRE}/RenderSystems/GL/src/OgreGLTexture.cpp"
  "${OGRE}/RenderSystems/GL/src/OgreGLTextureManager.cpp"
  "${OGRE}/RenderSystems/GL/src/win32/OgreWin32Context.cpp"
  "${OGRE}/RenderSystems/GL/src/OgreWin32GLSupport.cpp"
  "${OGRE}/RenderSystems/GL/src/OgreWin32RenderTexture.cpp"
  "${OGRE}/RenderSystems/GL/src/OgreWin32Window.cpp"
  "${OGRE}/RenderSystems/GL/src/nvparse/_ps1.0_lexer.cpp"
  "${OGRE}/RenderSystems/GL/src/nvparse/_ps1.0_parser.cpp"
  "${OGRE}/RenderSystems/GL/src/nvparse/_rc1.0_lexer.cpp"
  "${OGRE}/RenderSystems/GL/src/nvparse/_rc1.0_parser.cpp"
  "${OGRE}/RenderSystems/GL/src/nvparse/_ts1.0_lexer.cpp"
  "${OGRE}/RenderSystems/GL/src/nvparse/_ts1.0_parser.cpp"
  "${OGRE}/RenderSystems/GL/src/nvparse/_vs1.0_lexer.cpp"
  "${OGRE}/RenderSystems/GL/src/nvparse/_vs1.0_parser.cpp"
  "${OGRE}/RenderSystems/GL/src/nvparse/avp1.0_impl.cpp"
  "${OGRE}/RenderSystems/GL/src/nvparse/nvparse.cpp"
  "${OGRE}/RenderSystems/GL/src/nvparse/nvparse_errors.cpp"
  "${OGRE}/RenderSystems/GL/src/nvparse/ps1.0_program.cpp"
  "${OGRE}/RenderSystems/GL/src/nvparse/rc1.0_combiners.cpp"
  "${OGRE}/RenderSystems/GL/src/nvparse/rc1.0_final.cpp"
  "${OGRE}/RenderSystems/GL/src/nvparse/rc1.0_general.cpp"
  "${OGRE}/RenderSystems/GL/src/nvparse/ts1.0_inst.cpp"
  "${OGRE}/RenderSystems/GL/src/nvparse/ts1.0_inst_list.cpp"
  "${OGRE}/RenderSystems/GL/src/nvparse/vcp1.0_impl.cpp"
  "${OGRE}/RenderSystems/GL/src/nvparse/vp1.0_impl.cpp"
  "${OGRE}/RenderSystems/GL/src/nvparse/vs1.0_inst.cpp"
  "${OGRE}/RenderSystems/GL/src/nvparse/vs1.0_inst_list.cpp"
  "${OGRE}/RenderSystems/GL/src/nvparse/vsp1.0_impl.cpp"
  "${OGRE}/RenderSystems/GL/src/atifs/src/ATI_FS_GLGpuProgram.cpp"
  "${OGRE}/RenderSystems/GL/src/atifs/src/Compiler2Pass.cpp"
  "${OGRE}/RenderSystems/GL/src/atifs/src/ps_1_4.cpp"
  "${OGRE}/RenderSystems/GL/src/GLSL/src/OgreGLSLExtSupport.cpp"
  "${OGRE}/RenderSystems/GL/src/GLSL/src/OgreGLSLGpuProgram.cpp"
  "${OGRE}/RenderSystems/GL/src/GLSL/src/OgreGLSLLinkProgram.cpp"
  "${OGRE}/RenderSystems/GL/src/GLSL/src/OgreGLSLLinkProgramManager.cpp"
  "${OGRE}/RenderSystems/GL/src/GLSL/src/OgreGLSLProgram.cpp"
  "${OGRE}/RenderSystems/GL/src/GLSL/src/OgreGLSLProgramFactory.cpp"
)

set(OPENRBX_D3D9_SOURCES
  "${OGRE}/RenderSystems/Direct3D9/src/OgreD3D9Driver.cpp"
  "${OGRE}/RenderSystems/Direct3D9/src/OgreD3D9DriverList.cpp"
  "${OGRE}/RenderSystems/Direct3D9/src/OgreD3D9EngineDll.cpp"
  "${OGRE}/RenderSystems/Direct3D9/src/OgreD3D9GpuProgram.cpp"
  "${OGRE}/RenderSystems/Direct3D9/src/OgreD3D9GpuProgramManager.cpp"
  "${OGRE}/RenderSystems/Direct3D9/src/OgreD3D9HardwareBufferManager.cpp"
  "${OGRE}/RenderSystems/Direct3D9/src/OgreD3D9HardwareIndexBuffer.cpp"
  "${OGRE}/RenderSystems/Direct3D9/src/OgreD3D9HardwareOcclusionQuery.cpp"
  "${OGRE}/RenderSystems/Direct3D9/src/OgreD3D9HardwarePixelBuffer.cpp"
  "${OGRE}/RenderSystems/Direct3D9/src/OgreD3D9HardwareVertexBuffer.cpp"
  "${OGRE}/RenderSystems/Direct3D9/src/OgreD3D9HLSLProgram.cpp"
  "${OGRE}/RenderSystems/Direct3D9/src/OgreD3D9HLSLProgramFactory.cpp"
  "${OGRE}/RenderSystems/Direct3D9/src/OgreD3D9Mappings.cpp"
  "${OGRE}/RenderSystems/Direct3D9/src/OgreD3D9MultiRenderTarget.cpp"
  "${OGRE}/RenderSystems/Direct3D9/src/OgreD3D9Plugin.cpp"
  "${OGRE}/RenderSystems/Direct3D9/src/OgreD3D9RenderSystem.cpp"
  "${OGRE}/RenderSystems/Direct3D9/src/OgreD3D9RenderWindow.cpp"
  "${OGRE}/RenderSystems/Direct3D9/src/OgreD3D9Texture.cpp"
  "${OGRE}/RenderSystems/Direct3D9/src/OgreD3D9TextureManager.cpp"
  "${OGRE}/RenderSystems/Direct3D9/src/OgreD3D9VertexDeclaration.cpp"
  "${OGRE}/RenderSystems/Direct3D9/src/OgreD3D9VideoMode.cpp"
  "${OGRE}/RenderSystems/Direct3D9/src/OgreD3D9VideoModeList.cpp"
)

add_library(scilexer SHARED ${OPENRBX_SCILEXER_SOURCES})
set_target_properties(scilexer PROPERTIES
  OUTPUT_NAME "SciLexer"
  MSVC_RUNTIME_LIBRARY "MultiThreaded"
  RUNTIME_OUTPUT_DIRECTORY "${PROJECT_BINARY_DIR}"
)
target_include_directories(scilexer PRIVATE "${SCINTILLA}/include" "${SCINTILLA}/src")
target_compile_definitions(scilexer PRIVATE NDEBUG WIN32 _WINDOWS _MBCS _USRDLL SCI_LEXER)
target_compile_options(scilexer PRIVATE /O1 /Ob1 /W4 /EHsc)
target_link_options(scilexer PRIVATE /INCREMENTAL:NO /OPT:NOWIN98)
target_link_libraries(scilexer PRIVATE imm32 comctl32)

set(OPENRBX_OGRE_COMPILE_OPTIONS /Ob2 /Oi /Ot /Oy /GT /GF /EHsc /GS- /Gy /fp:fast /GR /W3 /Wp64)
set(OPENRBX_OGRE_LINK_OPTIONS /INCREMENTAL:NO /OPT:REF /OPT:ICF)

add_library(rgmain SHARED ${OPENRBX_OGREMAIN_SOURCES})
target_include_directories(rgmain PRIVATE "${OGRE}/OgreMain/include" "${OGRE}/Dependencies/include")
target_compile_definitions(rgmain PRIVATE WIN32 NDEBUG _USRDLL _MT OGRE_NONCLIENT_BUILD FREEIMAGE_LIB _MBCS)
target_compile_options(rgmain PRIVATE /O2 ${OPENRBX_OGRE_COMPILE_OPTIONS})
target_precompile_headers(rgmain PRIVATE "${OGRE}/OgreMain/include/OgreStableHeaders.h")
target_link_options(rgmain PRIVATE ${OPENRBX_OGRE_LINK_OPTIONS} /SUBSYSTEM:CONSOLE /OPT:NOWIN98)
target_link_directories(rgmain PRIVATE "${OGRE}/Dependencies/lib/Release")
target_link_libraries(rgmain PRIVATE zziplib zlib freetype230 freeimage LibJPEG LibMNG LibPNG LibTIFF)

add_library(rgpar SHARED ${OPENRBX_PARTICLEFX_SOURCES})
target_include_directories(rgpar PRIVATE
  "${OGRE}/PlugIns/ParticleFX/include"
  "${OGRE}/OgreMain/include"
  "${OGRE}/Dependencies/include"
)
target_compile_definitions(rgpar PRIVATE WIN32 NDEBUG _WINDOWS _USRDLL OGRE_PARTICLEFXPLUGIN_EXPORTS)
target_compile_options(rgpar PRIVATE /O2 ${OPENRBX_OGRE_COMPILE_OPTIONS} /GL)
target_link_options(rgpar PRIVATE ${OPENRBX_OGRE_LINK_OPTIONS} /SUBSYSTEM:WINDOWS /OPT:NOWIN98 /LTCG)
target_link_libraries(rgpar PRIVATE rgmain)

add_library(rggl SHARED ${OPENRBX_GL_SOURCES})
target_include_directories(rggl PRIVATE
  "${OGRE}/RenderSystems/GL/include"
  "${OGRE}/RenderSystems/GL/src/win32"
  "${OGRE}/OgreMain/include"
  "${OGRE}/Dependencies/include"
  "${OGRE}/RenderSystems/GL/src/nvparse"
  "${OGRE}/RenderSystems/GL/src/nvparse/winheaders"
  "${OGRE}/RenderSystems/GL/src/atifs/include"
  "${OGRE}/RenderSystems/GL/src/glsl/include"
)
target_compile_definitions(rggl PRIVATE WIN32 NDEBUG _WINDOWS _USRDLL OGRE_GLPLUGIN_EXPORTS _MBCS)
target_compile_options(rggl PRIVATE /Ox ${OPENRBX_OGRE_COMPILE_OPTIONS} /GL)
target_link_options(rggl PRIVATE ${OPENRBX_OGRE_LINK_OPTIONS} /SUBSYSTEM:WINDOWS /LTCG)
target_link_libraries(rggl PRIVATE rgmain opengl32.lib glu32.lib)

add_library(rgdx SHARED ${OPENRBX_D3D9_SOURCES})
target_include_directories(rgdx PRIVATE
  "${OGRE}/RenderSystems/Direct3D9/include"
  "${OGRE}/OgreMain/include"
  "${OGRE}/Dependencies/include"
  "${DXSDK}/Include"
)
target_compile_definitions(rgdx PRIVATE WIN32 NDEBUG _WINDOWS _USRDLL OGRED3DENGINEDLL_EXPORTS _MBCS)
target_compile_options(rgdx PRIVATE /Ox ${OPENRBX_OGRE_COMPILE_OPTIONS} /GL)
target_link_options(rgdx PRIVATE ${OPENRBX_OGRE_LINK_OPTIONS} /SUBSYSTEM:WINDOWS /LTCG)
target_link_directories(rgdx PRIVATE "${DXSDK}/Lib")
target_link_libraries(rgdx PRIVATE rgmain d3d9 d3dx9 dxerr9 dxguid)

set_target_properties(rgmain rgpar rggl rgdx PROPERTIES
  VERSION 0.99
  MSVC_RUNTIME_LIBRARY "MultiThreadedDLL"
  RUNTIME_OUTPUT_DIRECTORY "${PROJECT_BINARY_DIR}"
)
