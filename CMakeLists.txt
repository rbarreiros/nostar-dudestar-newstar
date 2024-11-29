cmake_minimum_required(VERSION 3.16)
project(dudestar VERSION 1.0 LANGUAGES C CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOUIC ON)

find_package(QT NAMES Qt5 Qt6 REQUIRED COMPONENTS Core)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Gui Multimedia Network SerialPort)
find_package(Qt${QT_VERSION_MAJOR} OPTIONAL_COMPONENTS Widgets)
find_package(Flite)

qt_standard_project_setup()

qt_add_executable(dudestar WIN32 MACOSX_BUNDLE
    CRCenc.cpp CRCenc.h
    DMRData.cpp DMRData.h
    DMRDefines.h
    Golay24128.cpp Golay24128.h
    SHA256.cpp SHA256.h
    YSFConvolution.cpp YSFConvolution.h
    YSFFICH.cpp YSFFICH.h
    ambe.c ambe.h
    ambe3600x2400.c
    ambe3600x2400_const.h
    ambe3600x2450.c
    ambe3600x2450_const.h
    audioengine.cpp audioengine.h
    cbptc19696.cpp cbptc19696.h
    cgolay2087.cpp cgolay2087.h
    chamming.cpp chamming.h
    codec.cpp codec.h
    codec2/codebooks.cpp
    codec2/codec2.cpp codec2/codec2.h
    codec2/codec2_internal.h
    codec2/defines.h
    codec2/kiss_fft.cpp codec2/kiss_fft.h
    codec2/lpc.cpp codec2/lpc.h
    codec2/nlp.cpp codec2/nlp.h
    codec2/pack.cpp
    codec2/qbase.cpp codec2/qbase.h
    codec2/quantise.cpp codec2/quantise.h
    crs129.cpp crs129.h
    dcscodec.cpp dcscodec.h
    dmrcodec.cpp dmrcodec.h
    dudestar.cpp dudestar.h dudestar.ui
    ecc.c
    ecc_const.h
    httpmanager.cpp httpmanager.h
    iaxcodec.cpp iaxcodec.h
    iaxdefines.h
    imbe7200x4400.c
    imbe7200x4400_const.h
    imbe_vocoder/aux_sub.cc imbe_vocoder/aux_sub.h
    imbe_vocoder/basic_op.h
    imbe_vocoder/basicop2.cc
    imbe_vocoder/ch_decode.cc imbe_vocoder/ch_decode.h
    imbe_vocoder/ch_encode.cc imbe_vocoder/ch_encode.h
    imbe_vocoder/dc_rmv.cc imbe_vocoder/dc_rmv.h
    imbe_vocoder/decode.cc imbe_vocoder/decode.h
    imbe_vocoder/dsp_sub.cc imbe_vocoder/dsp_sub.h
    imbe_vocoder/encode.cc imbe_vocoder/encode.h
    imbe_vocoder/globals.h
    imbe_vocoder/imbe.h
    imbe_vocoder/imbe_vocoder.cc imbe_vocoder/imbe_vocoder.h
    imbe_vocoder/math_sub.cc imbe_vocoder/math_sub.h
    imbe_vocoder/pe_lpf.cc imbe_vocoder/pe_lpf.h
    imbe_vocoder/pitch_est.cc imbe_vocoder/pitch_est.h
    imbe_vocoder/pitch_ref.cc imbe_vocoder/pitch_ref.h
    imbe_vocoder/qnt_sub.cc imbe_vocoder/qnt_sub.h
    imbe_vocoder/rand_gen.cc imbe_vocoder/rand_gen.h
    imbe_vocoder/sa_decode.cc imbe_vocoder/sa_decode.h
    imbe_vocoder/sa_encode.cc imbe_vocoder/sa_encode.h
    imbe_vocoder/sa_enh.cc imbe_vocoder/sa_enh.h
    imbe_vocoder/tbls.cc imbe_vocoder/tbls.h
    imbe_vocoder/typedef.h
    imbe_vocoder/typedefs.h
    imbe_vocoder/uv_synt.cc imbe_vocoder/uv_synt.h
    imbe_vocoder/v_synt.cc imbe_vocoder/v_synt.h
    imbe_vocoder/v_uv_det.cc imbe_vocoder/v_uv_det.h
    levelmeter.cpp levelmeter.h
    m17codec.cpp m17codec.h
    main.cpp
    mbedec.cpp mbedec.h
    mbeenc.cc mbeenc.h
    mbelib.c
    mbelib_const.h
    mbelib_parms.h
    nxdncodec.cpp nxdncodec.h
    p25codec.cpp p25codec.h
    refcodec.cpp refcodec.h
    serialambe.cpp serialambe.h
    serialmodem.cpp serialmodem.h
    vocoder_tables.h
    xrfcodec.cpp xrfcodec.h
    ysfcodec.cpp ysfcodec.h
)
target_compile_definitions(dudestar PRIVATE
    AMBEHW_SUPPORTED
    QT_DEPRECATED_WARNINGS
    USE_SWTX
    VERSION_NUMBER=\"\\\"'$\\\"\"
)

target_link_libraries(dudestar PRIVATE
    Qt::Core
    Qt::Gui
    Qt::Multimedia
    Qt::Network
    Qt::SerialPort
)


# Resources:
set(dudestar_resource_files
    "images/dudestar.png"
)

qt_add_resources(dudestar "dudestar"
    PREFIX
        "/"
    FILES
        ${dudestar_resource_files}
)

if((QT_VERSION_MAJOR GREATER 4))
    target_link_libraries(dudestar PRIVATE
        Qt::Widgets
    )
endif()

if(MACOS)
    target_sources(dudestar PUBLIC
        micpermission.h micpermission.mm
    )

    target_link_libraries(dudestar PRIVATE
        "-framework AVFoundation"
    )
endif()

if(DEFINES___contains___USE_FLITE)
    target_link_libraries(dudestar PRIVATE
        Flite::Flite
        asound
        flite_cmu_us_awb
        flite_cmu_us_kal16
        flite_cmu_us_rms
        flite_cmu_us_slt
        flite_cmulex
        flite_usenglish
    )
endif()

install(TARGETS dudestar
    BUNDLE DESTINATION .
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

#qt_generate_deploy_app_script(
#    TARGET dudestar
#    FILENAME_VARIABLE deploy_script
#    NO_UNSUPPORTED_PLATFORM_ERROR
#)
#install(SCRIPT ${deploy_script})
