INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR})


include (TestBigEndian)
TEST_BIG_ENDIAN(IS_BIG_ENDIAN)
if(IS_BIG_ENDIAN)
add_definitions(-DOSC_HOST_BIG_ENDIAN)
else()
add_definitions(-DOSC_HOST_LITTLE_ENDIAN)
endif()



# separate versions of NetworkingUtils.cpp and UdpSocket.cpp are provided for Win32 and POSIX
# the IpSystemTypePath selects the correct ones based on the current platform

IF(WIN32)
 set(IpSystemTypePath ip/win32)
 set(OSCPACK_EXTRA_LIBS ${OSCPACK_EXTRA_LIBS} Ws2_32 winmm)
ELSE(WIN32)
 set(IpSystemTypePath ip/posix)
ENDIF(WIN32)

ADD_LIBRARY(oscpack STATIC

ip/IpEndpointName.h
ip/IpEndpointName.cpp

ip/NetworkingUtils.h
${IpSystemTypePath}/NetworkingUtils.cpp

ip/UdpSocket.h
${IpSystemTypePath}/UdpSocket.cpp

ip/PacketListener.h
ip/TimerListener.h

osc/OscTypes.h
osc/OscTypes.cpp 
osc/OscHostEndianness.h
osc/OscException.h
osc/OscPacketListener.h
osc/MessageMappingOscPacketListener.h
osc/OscReceivedElements.h
osc/OscReceivedElements.cpp
osc/OscPrintReceivedElements.h
osc/OscPrintReceivedElements.cpp
osc/OscOutboundPacketStream.h
osc/OscOutboundPacketStream.cpp

)

target_link_libraries(oscpack ${OSCPACK_EXTRA_LIBS})
