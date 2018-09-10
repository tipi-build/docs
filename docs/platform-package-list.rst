
.. to generate the list: 
.. ls -1 | while read line; do echo $line; fgrep target_link_libraries $line; done

.. _package-list:

Platform Package List
=====================
We rely on the https://docs.hunter.sh/en/latest/ project for the platform dependencies, you can see the version of the platform libraries used by default by ``nxxm`` `in it's Hunter config <https://github.com/nxxm/hunter/blob/nxxm/v0.0.3/cmake/configs/default.cmake>`_.

* accelerate:
    - ``accelerate::accelerate``
* acf: 
    - ``acf acf::acf``
* aes:
    - ``aes::aes``
* aglet:
    - ``aglet::aglet``
* AllTheFlopsThreads:
* Android-Apk:
* android_arm64_v8a_system_image_packer:
* Android-ARM64-v8a-System-Image:
* android_arm_eabi_v7a_system_image_packer:
* Android-ARM-EABI-v7a-System-Image:
* android_build_tools_packer:
* Android-Build-Tools:
* android_google_apis_intel_x86_atom_system_image_packer:
* Android-Google-APIs-Intel-x86-Atom-System-Image:
* android_google_apis_packer:
* Android-Google-APIs:
* android_google_repository_packer:
* Android-Google-Repository:
* android_intel_x86_atom_system_image_packer:
* Android-Intel-x86-Atom-System-Image:
* android_log:
    - ``android_log::android_log``
* android_mips_system_image_packer:
* Android-MIPS-System-Image:
* Android-Modules:
* android:
    - ``android::android``
* android_sdk_packer:
* android_sdk_platform_packer:
* Android-SDK-Platform:
* android_sdk_platform_tools_packer:
* Android-SDK-Platform-tools:
* Android-SDK:
* android_sdk_tools_packer:
* Android-SDK-Tools:
* android_support_repository_packer:
* Android-Support-Repository:
* AngelScript:
    - ``boo PUBLIC AngelScript::AngelScript``
* appkit:
    - ``appkit::appkit``
    - ``"-framework AppKit"``
* ARM_NEON_2_x86_SSE:
    - ``ARM_NEON_2_x86_SSE::ARM_NEON_2_x86_SSE``
* ArrayFire:
    - ``ArrayFire::af``
    - ``ArrayFire::afcpu``
* assetslibrary:
    - ``assetslibrary::assetslibrary``
    - ``"-framework AssetsLibrary"``
* Assimp:
    - ``Assimp::assimp``
* Async++:
    - ``Async++::Async++``
* audiotoolbox:
    - ``audiotoolbox::audiotoolbox``
    - ``"-framework AudioToolbox"``
* audiounit:
    - ``audiounit::audiounit``
    - ``"-framework AudioUnit"``
* autobahn-cpp:
    - ``autobahn-cpp::autobahn-cpp``
* autoutils:
* Avahi:
    - ``Avahi::common Avahi::client Avahi::compat_libdns_sd``
* avfoundation:
    - ``avfoundation::avfoundation``
    - ``"-framework AVFoundation"``
* Beast:
    - ``Beast::Beast``
* benchmark:
    - ``benchmark::benchmark``
* bison:
* BoostCompute:
    - ``BoostCompute::boost_compute``
* boost-pba:
    - ``boost-pba::boost-pba``
* BoostProcess:
    - ``BoostProcess::boost_process``
* Boost:
    - ``Boost::+boost``
    - ``Boost::system Boost::filesystem``
    - ``Boost::`` followed by any Boost Library name.
* BoringSSL:
    - ``boo BoringSSL::ssl BoringSSL::crypto``
* Box2D:
    - ``boo PUBLIC Box2D::Box2D``
* bullet:
    - ````
* BZip2:
    - ``BZip2::bz2``
* caffe:
    - ``caffe``
* CapnProto:
    - ``CapnProto::capnp``
* carbon:
    - ``carbon::carbon``
    - ``"-framework Carbon"``
* c-ares:
    - ``c-ares::cares``
* Catch:
* catkin:
* cctz:
* ccv:
    - ``ccv::ccv``
* cereal:
    - ``cereal::cereal``
* ceres-solver:
    - ``PRIVATE ceres``
* check_ci_tag:
* civetweb:
    - ``boo PUBLIC civetweb::c-library``
* Clang:
* ClangToolsExtra:
* CLAPACK:
* clBLAS:
* CLI11:
* Comet:
    - ``Comet::comet``
* convertutf:
    - ``convertutf::convertutf``
* coreaudio:
    - ``coreaudio::coreaudio``
    - ``"-framework CoreAudio"``
* coredata:
    - ``coredata::coredata``
    - ``"-framework CoreData"``
* corefoundation:
    - ``corefoundation::corefoundation``
    - ``"-framework CoreFoundation"``
* coregraphics:
    - ``coregraphics::coregraphics``
    - ``"-framework CoreGraphics"``
* corelocation:
    - ``corelocation::corelocation``
    - ``"-framework CoreLocation"``
* coremedia:
    - ``coremedia::coremedia``
    - ``"-framework CoreMedia"``
* coremotion:
    - ``coremotion::coremotion``
    - ``"-framework CoreMotion"``
* corevideo:
    - ``corevideo::corevideo``
    - ``"-framework CoreVideo"``
* CppNetlib:
* CppNetlibUri:
    - ``network-uri``
* cpp_redis:
    - ``cpp_redis::cpp_redis``
* cpr:
    - ``cpr::cpr``
* crashpad:
    - ``   crashpad::crashpad_client``
* crashup:
* crc32c:
    - ``crc32c::crc32c``
* cryptopp:
    - ``cryptopp-static``
* CsvParserCPlusPlus:
    - ``CsvParserCPlusPlus::csv_parser_cplusplus``
* ctti:
* cub:
    - ``cub::cub``
* CURL:
    - ``CURL::libcurl``
* cvmatio:
    - ``cvmatio::cvmatio``
* cvsteer:
* cxxopts:
* czmq:
* damageproto:
* date:
* dbus:
* debug_assert:
    - ``debug_assert_example debug_assert``
* dest:
    - ``dest::dest``
* dlib:
* dmlc-core:
* doctest:
    - ``doctest::doctest``
* double-conversion:
    - ``double-conversion::double-conversion``
* dri2proto:
* dri3proto:
* drishti_assets:
* drishti_faces:
* drishti:
* drm:
* duktape:
* dynalo:
* egl:
    - ``egl::egl``
* eigen3-nnls:
* Eigen:
    - ``Eigen::eigen``
* enet:
* EnumGroup:
* eos:
    - ``eos::eos``
* ethash:
* Expat:
    - ``${EXPAT_LIBRARIES}``
* FakeIt:
    - ``FakeIt::FakeIt``
* farmhash:
    - ``farmhash farmhash::farmhash``
* fft2d:
    - ``fft2d fft2d::fft2d``
* fixesproto:
* flatbuffers:
    - ``flatbuffers::flatbuffers``
* flex:
    - ``main ${FLEX_LIBRARIES}``
    - ``main ${BISON_LIBRARIES} ${FLEX_LIBRARIES}``
* fmt:
    - ``fmt``
* folly:
* :
* forcefeedback:
    - ``forcefeedback::forcefeedback``
    - ``"-framework ForceFeedback"``
* foundation:
    - ``foundation::foundation``
    - ``"-framework Foundation"``
* freetype:
    - ``freetype::freetype``
* frugally-deep:
* Fruit:
* FunctionalPlus:
* gamecontroller:
    - ``gamecontroller::gamecontroller``
    - ``"-framework GameController"``
* gauze:
* gemmlowp:
    - ``gemmlowp gemmlowp::gemmlowp``
* geos:
* getopt:
* gflags:
    - ``gflags``
* giflib:
    - ``giflib giflib::giflib``
* glapi:
    - ``glapi::glapi``
* glbinding:
    - ``glbinding glbinding::glbinding``
* gles2:
    - ``gles2::gles2``
* gles3:
    - ``gles3::gles3``
* glew:
    - ``boo PUBLIC glew::glew``
* glfw:
    - ``glfw``
* glib:
    - ``PkgConfig::glib-2.0``
* glkit:
    - ``glkit::glkit``
    - ``"-framework GLKit"``
* glm:
    - ``PRIVATE glm``
* globjects:
    - ``globjects::globjects``
* glog:
    - ``glog::glog``
    - ``glog``
* glproto:
* glslang:
* GPUImage:
* gRPC:
    - ``gRPC::grpc``
* GSL:
    - ``GSL::gsl``
* gst_plugins_bad:
    - ``PkgConfig::gstreamer-bad-video-1.0``
* gst_plugins_base:
    - ``PkgConfig::gstreamer-video-1.0``
* gst_plugins_good:
* gst_plugins_ugly:
* gstreamer:
    - ``PkgConfig::gstreamer-1.0``
* GTest:
    - ``GTest::main) # GTest::gtest will be linked automatically``
    - ``boo GTest::gtest``
    - ``GMock::main``
* gumbo:
    - ``gumbo::gumbo``
* h3:
* half:
    - ``half::half``
* harfbuzz:
* hdf5:
    - ``hdf5``
* highwayhash:
    - ``highwayhash highwayhash::highwayhash``
* http-parser:
* ice:
* ICU:
* IF97:
    - ``IF97 IF97::IF97``
* Igloo:
* imageio:
    - ``imageio::imageio``
    - ``"-framework ImageIO"``
* imgui:
* imshow:
    - ``imshow::imshow``
* inja:
    - ``inja inja::inja``
* inputproto:
* intltool:
* intsizeof:
    - ``PUBLIC intsizeof::intsizeof``
* iokit:
    - ``iokit::iokit``
    - ``"-framework IOKit"``
* ios_sim:
* ippicv:
* irrXML:
    - ``irrXML::irrXML``
* jaegertracing:
* jansson:
* jasper:
* jo_jpeg:
    - ``jo_jpeg::jo_jpeg``
* Jpeg:
* jsoncpp:
    - ``jsoncpp_lib_static``
* JsonSpirit:
    - ``json``
* kbproto:
* kNet:
    - ``boo PUBLIC kNet::kNet``
* LAPACK:
    - ``blas lapack``
* lcms:
* Leathers:
* Leptonica:
* leveldb:
    - ``leveldb::leveldb``
* LibCDS:
    - ``LibCDS::cds)  # Use cds-s for static library``
* libcpuid:
    - ``boo PUBLIC libcpuid::libcpuid``
* Libcxxabi:
* Libcxx:
* libdaemon:
* libdill:
    - ``libdill libdill::dill``
* Libevent:
    - ``Libevent::event_core``
* libevhtp:
* libffi:
    - ``PkgConfig::libffi``
* libjson-rpc-cpp:
* libmill:
    - ``libmill libmill::mill_s``
* libogg:
* libpcre:
    - ``PkgConfig::libpcre``
* librtmp:
* libscrypt:
* libsodium:
    - ``libsodium::libsodium``
* Libssh2:
* libunibreak:
* libuv:
    - ``libuv::uv``
* libxml2:
* libyuv:
    - ``PUBLIC libyuv::yuv``
* LLVMCompilerRT:
* LLVM:
* lmdb:
    - ``lmdb liblmdb::lmdb``
* lmdbxx:
    - ``lmdbxx::lmdbxx``
* log4cplus:
    - ``log4cplus::log4cplus``
* Lua:
* lz4:
    - ``boo PUBLIC lz4::lz4``
* lzma:
    - ``lzma::lzma``
* md5:
    - ``boo PUBLIC md5::md5``
* metal:
    - ``metal::metal``
    - ``"-framework Metal"``
Microsoft.* GSL:
* mini_chromium:
* minizip:
    - ``minizip::minizip``
* mng:
* mobilecoreservices:
    - ``mobilecoreservices::mobilecoreservices``
    - ``"-framework MobileCoreServices"``
* mojoshader:
    - ``boo PUBLIC mojoshader::mojoshader``
* mongoose:
    - ``mongoose mongoose::mongoose``
* mpark_variant:
* msgpack:
    - ``msgpack::msgpack``
* mtplz:
* MySQL-client:
    - ``"MySQL::libmysql"``
    - ``"MySQL::client"``
* nanoflann:
* NASM:
* ncnn:
* nlohmann_json:
    - ``nlohmann_json``
    - ``nlohmann-json::nlohmann-json``
-  shorten ``)``to
   ``nlohmann_json)``**no**
* nsync:
    - ``nsync::nsync``
* odb-boost:
* odb-compiler:
* odb-mysql:
* odb-pgsql:
    - ``odb::pgsql``
* odb:
* odb-sqlite:
* ogles_gpgpu:
    - ``ogles_gpgpu::ogles_gpgpu``
* oniguruma:
* onmt:
* OpenAL:
    - ``OpenAL::OpenAL``
* OpenBLAS:
    - ``OpenBLAS::OpenBLAS``
* OpenCL-cpp:
    - ``PRIVATE OpenCL-cpp::OpenCL-cpp``
* OpenCL:
    - ``PRIVATE OpenCL::OpenCL``
* OpenCV-Extra:
* OpenCV:
    - ``PRIVATE ${OpenCV_LIBS}``
* openddlparser:
    - ``openddlparser::openddl_parser``
* opengles:
    - ``opengles::opengles``
    - ``"-framework OpenGLES"``
* OpenNMTTokenizer:
* OpenSSL:
    - ``OpenSSL::+SSL OpenSSL::+Crypto``
* opentracing-cpp:
    - ``OpenTracing::opentracing``
    - ``OpenTracing::opentracing-static``
* osmesa:
    - ``osmesa::osmesa``
* pcg:
* pciaccess:
* PhysUnits:
* PNG:
* PocoCpp:
    - ``Poco::Foundation``
* poly2tri:
    - ``poly2tri::poly2tri``
* polyclipping:
    - ``polyclipping::polyclipping``
* PostgreSQL:
    - ``PostgreSQL::libpq``
* presentproto:
* PROJ4:
* protobuf-c:
* Protobuf:
    - ``protobuf::libprotobuf``
* pthread-stubs:
* pugixml:
    - ``boo PUBLIC pugixml``
* pybind11:
    - ``pybind11::pybind11 pybind11::embed pybind11::module``
* QtAndroidCMake:
* QtCMakeExtra:
* QtQmlManager:
* Qt:
* quartzcore:
    - ``quartzcore::quartzcore``
    - ``"-framework QuartzCore"``
* rabbitmq-c:
    - ``rabbitmq-c::rabbitmq-static``
* rabit:
* randrproto:
* range-v3:
* RapidJSON:
    - ``RapidJSON::rapidjson``
* RapidXML:
    - ``RapidXML::RapidXML``
* re2:
    - ``RE2::re2``
* recastnavigation:
    - ````
* renderproto:
* rocksdb:
* ros_comm_msgs:
* ros_common_msgs:
* ros_console_bridge:
* roscpp_core:
* ros_environment:
* ros_gencpp:
* ros_geneus:
* ros_genlisp:
* ros_genmsg:
* ros_gennodejs:
* ros_genpy:
* ros_message_generation:
* ros_message_runtime:
* rospack:
* ros:
* ros_std_msgs:
* SDL2:
    - ``SDL2::SDL2``
* SDL_image:
    - ``main``
* SDL_mixer:
    - ``SDL_mixer::SDL_mixer``
* SDL_ttf:
    - ``SDL_ttf::SDL_ttf``
* sds:
    - ``sds::sds``
* sm:
* Snappy:
* Sober:
* sources_for_android_sdk_packer:
* Sources-for-Android-SDK:
* sparsehash:
* spdlog:
    - ``spdlog::spdlog``
* sqlite3:
* sse2neon:
    - ``sse2neon::sse2neon``
* stanhull:
    - ``boo PUBLIC stanhull::stanhull``
* state_machine:
    - ``   sm state_machine``
* stb:
    - ``boo PUBLIC stb::stb``
* stdext-path:
* stormlib:
    - ``stormlib::stormlib``
* sugar:
* SuiteSparse:
    - ``SuiteSparse::cholmod``
* szip:
    - ``szip::szip``
* tacopie:
    - ``tacopie::tacopie``
* tclap:
* tcl:
* Tesseract:
* thread-pool-cpp:
    - ``thread-pool-cpp::thread-pool-cpp``
* thrift:
    - ``PUBLIC``
* TIFF:
    - ``TIFF::libtiff``
* tinydir:
    - ``tinydir::tinydir``
* tinyxml2:
* toluapp:
* tomcrypt:
* tommath:
* type_safe:
    - ``type_safe_example type_safe``
* uikit:
    - ``uikit::uikit``
    - ``"-framework UIKit"``
* Urho3D:
    - ``boo PUBLIC Urho3D::Urho3D``
* util_linux:
    - ````
* vorbis:
* VulkanMemoryAllocator:
* Washer:
* WDC:
    - ``WDC::libwdc``
* WebKit:
* WebP:
* websocketpp:
    - ``websocketpp::websocketpp``
* WinSparkle:
* WTL:
    - ``WTL::WTL``
* wxWidgets:
    - ``${wxWidgets_LIBRARIES}``
* x11:
* x264:
* xau:
* xcb-proto:
* xcb:
* xcursor:
* xdamage:
* xextproto:
* xext:
* xf86vidmodeproto:
* xfixes:
* xgboost:
    - ``xgboost::xgboost``
* xineramaproto:
* xinerama:
* xi:
* xorg-macros:
* xproto:
* xrandr:
* xrender:
* xshmfence:
* xtrans:
* xxf86vm:
* yaml-cpp:
    - ``yaml-cpp::yaml-cpp``
* ZeroMQ:
    - ``ZeroMQ::libzmq) ``
* ZLIB:
    - ``ZLIB::zlib``
* ZMQPP:
    - ``ZMQPP::zmqpp``
* zookeeper:
    - ``zookeeper::zookeeper_mt``
    - ``# zookeeper::zookeeper_st) # if you want the single-threaded lib instead``
