<<<<<<< HEAD
set(SERVER_SOURCES
    config.cpp
    controlServer.cpp
    controlSession.cpp
    snapServer.cpp
    streamServer.cpp
    streamSession.cpp
    encoder/encoderFactory.cpp
    encoder/pcmEncoder.cpp
    streamreader/streamUri.cpp
    streamreader/streamManager.cpp
    streamreader/pcmStream.cpp
    streamreader/pipeStream.cpp
    streamreader/fileStream.cpp
    streamreader/airplayStream.cpp
    streamreader/spotifyStream.cpp
    streamreader/watchdog.cpp
    streamreader/processStream.cpp)

set(SERVER_LIBRARIES
    ${CMAKE_THREAD_LIBS_INIT}
    ${JSONRPCPP_LIBRARIES}
    ${FLAC_LIBRARIES}
    common message)

set(SERVER_INCLUDE
    ${CMAKE_SOURCE_DIR}/server
    ${JSONRPCPP_INCLUDE_DIRS}
    ${FLAC_INCLUDE_DIRS})

# Avahi
if (AVAHI_FOUND)
    list(APPEND SERVER_SOURCES publishZeroConf/publishAvahi.cpp)
    list(APPEND SERVER_LIBRARIES ${AVAHI_LIBRARIES})
    list(APPEND SERVER_INCLUDE ${AVAHI_INCLUDE_DIRS})
endif (AVAHI_FOUND)

if (BONJOUR_FOUND)
    list(APPEND SERVER_SOURCES publishZeroConf/publishBonjour.cpp)
#    list(APPEND SERVER_LIBRARIES ${BONJOUR_LIBRARIES})
#    list(APPEND SERVER_INCLUDE ${BONJOUR_INCLUDE_DIRS})
endif (BONJOUR_FOUND)

if (OGG_FOUND AND VORBIS_FOUND AND VORBISENC_FOUND)
    list(APPEND SERVER_SOURCES encoder/oggEncoder.cpp)
    list(APPEND SERVER_LIBRARIES
        ${OGG_LIBRARIES}
        ${VORBIS_LIBRARIES}
        ${VORBISENC_LIBRARIES})
    list(APPEND SERVER_INCLUDE
        ${OGG_INCLUDE_DIRS}
        ${VORBIS_INCLUDE_DIRS}
        ${VORBISENC_INCLUDE_DIRS})
endif (OGG_FOUND AND VORBIS_FOUND AND VORBISENC_FOUND)

if (FLAC_FOUND)
    list(APPEND SERVER_SOURCES encoder/flacEncoder.cpp)
    list(APPEND SERVER_LIBRARIES ${FLAC_LIBRARIES})
    list(APPEND SERVER_INCLUDE ${FLAC_INCLUDE_DIRS})
endif (FLAC_FOUND)
=======
set(SERVER_SOURCES snapServer.cpp config.cpp controlServer.cpp controlSession.cpp streamServer.cpp streamSession.cpp json/jsonrpc.cpp streamreader/streamUri.cpp streamreader/streamManager.cpp streamreader/pcmStream.cpp streamreader/pipeStream.cpp streamreader/fileStream.cpp encoder/encoderFactory.cpp encoder/flacEncoder.cpp encoder/pcmEncoder.cpp encoder/oggEncoder.cpp streamreader/airplayStream.cpp streamreader/spotifyStream.cpp streamreader/watchdog.cpp streamreader/processStream.cpp)

set(SERVER_LIBRARIES ${CMAKE_THREAD_LIBS_INIT} ${FLAC_LIBRARIES} ${OGG_LIBRARIES} ${VORBIS_LIBRARIES} ${VORBISENC_LIBRARIES} common message)

set(SERVER_INCLUDE ${CMAKE_SOURCE_DIR} ${CMAKE_SOURCE_DIR}/externals/asio/asio/include ${CMAKE_SOURCE_DIR}/externals/popl/include ${CMAKE_SOURCE_DIR}/server ${FLAC_INCLUDE_DIRS} ${VORBIS_INCLUDE_DIRS} ${OGG_INCLUDE_DIRS})

if (APPLE)
  list(APPEND SERVER_SOURCES publishZeroConf/publishBonjour.cpp)
elseif (UNIX)
  list(APPEND SERVER_SOURCES publishZeroConf/publishAvahi.cpp)
  list(APPEND SERVER_LIBRARIES ${AVAHI_LIBRARIES} rt)
  list(APPEND SERVER_INCLUDE ${AVAHI_INCLUDE_DIRS})
endif()
>>>>>>> 281f1bd161fd2a4cd4aa11daf08ca5c3fe88d83d

include_directories(${SERVER_INCLUDE})
add_executable(snapserver ${SERVER_SOURCES})
target_link_libraries(snapserver ${SERVER_LIBRARIES})

<<<<<<< HEAD
install(TARGETS snapserver COMPONENT server DESTINATION "${CMAKE_INSTALL_BINDIR}")
=======
install(TARGETS snapserver COMPONENT server DESTINATION bin)
if(UNIX)
  install(FILES snapserver.1 COMPONENT server DESTINATION share/man/man1/snapserver.1)
  install(FILES ${CMAKE_SOURCE_DIR}/platform/debian/server/copyright COMPONENT server DESTINATION usr/share/doc/snapserver)
  install(FILES ${CMAKE_SOURCE_DIR}/platform/debian/server/snapserver.service COMPONENT server DESTINATION lib/systemd/system)
  install(FILES ${CMAKE_SOURCE_DIR}/platform/debian/server/snapserver.init COMPONENT server DESTINATION etc/init.d RENAME snapserver)
  install(FILES ${CMAKE_SOURCE_DIR}/platform/debian/server/snapserver.default COMPONENT server DESTINATION etc/default RENAME snapserver)
  install(FILES ${CMAKE_CURRENT_BINARY_DIR}/../README.md.gz COMPONENT server DESTINATION share/doc/snapserver)
  install(FILES ${CMAKE_CURRENT_BINARY_DIR}/../changelog.gz COMPONENT server DESTINATION share/doc/snapserver)
endif()
>>>>>>> 281f1bd161fd2a4cd4aa11daf08ca5c3fe88d83d
