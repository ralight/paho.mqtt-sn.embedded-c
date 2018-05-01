#*******************************************************************************
#  Copyright (c) 2017 IBM Corp.
#
#  All rights reserved. This program and the accompanying materials
#  are made available under the terms of the Eclipse Public License v1.0
#  and Eclipse Distribution License v1.0 which accompany this distribution.
#
#  The Eclipse Public License is available at
#     http://www.eclipse.org/legal/epl-v10.html
#  and the Eclipse Distribution License is available at
#    http://www.eclipse.org/org/documents/edl-v10.php.
#
#  Contributors:
#     Ian Craggs - initial version
#*******************************************************************************/


PROJECT(mqtt-sn-gateway C)

SET(SRCDIR src)
SET(SUBDIR ../MQTTSNPacket/src)
SET(TEST tests)
SET(OS linux)

SET(CPPSRCS
	${SRCDIR}/MQTTGWConnectionHandler.cpp
	${SRCDIR}/MQTTGWPacket.cpp
	${SRCDIR}/MQTTGWPublishHandler.cpp
	${SRCDIR}/MQTTGWSubscribeHandler.cpp
	${SRCDIR}/MQTTSNGateway.cpp
	${SRCDIR}/MQTTSNGWBrokerRecvTask.cpp
	${SRCDIR}/MQTTSNGWBrokerSendTask.cpp
	${SRCDIR}/MQTTSNGWClient.cpp
	${SRCDIR}/MQTTSNGWClientRecvTask.cpp
	${SRCDIR}/MQTTSNGWClientSendTask.cpp
	${SRCDIR}/MQTTSNGWConnectionHandler.cpp
	${SRCDIR}/MQTTSNGWLogmonitor.cpp
	${SRCDIR}/MQTTSNGWPacket.cpp
	${SRCDIR}/MQTTSNGWPacketHandleTask.cpp
	${SRCDIR}/MQTTSNGWProcess.cpp
	${SRCDIR}/MQTTSNGWPublishHandler.cpp
	${SRCDIR}/MQTTSNGWSubscribeHandler.cpp
	${SRCDIR}/${OS}/Timer.cpp 
	${SRCDIR}/${OS}/Network.cpp
	${SRCDIR}/${OS}/Threading.cpp
)

SET(TESTSRCS
	${SRCDIR}/${TEST}/TestProcess.cpp
	${SRCDIR}/${TEST}/TestQue.cpp
	${SRCDIR}/${TEST}/TestTree23.cpp
	${SRCDIR}/${TEST}/TestTopics.cpp
	${SRCDIR}/${TEST}/TestTopicIdMap.cpp
	${SRCDIR}/${TEST}/TestTask.cpp
)

SET(CSRCS
	${SUBDIR}/MQTTSNConnectClient.c
	${SUBDIR}/MQTTSNConnectServer.c
	${SUBDIR}/MQTTSNDeserializePublish.c
	${SUBDIR}/MQTTSNPacket.c
	${SUBDIR}/MQTTSNSearchClient.c
	${SUBDIR}/MQTTSNSearchServer.c
	${SUBDIR}/MQTTSNSerializePublish.c
	${SUBDIR}/MQTTSNSubscribeClient.c
	${SUBDIR}/MQTTSNSubscribeServer.c
	${SUBDIR}/MQTTSNUnsubscribeClient.c
	${SUBDIR}/MQTTSNUnsubscribeServer.c 
)

SET(CMAKE_CXX_FLAGS
	"-Wall -O3 -std=c++11"
)

# =========================== UDP ===========================
INCLUDE_DIRECTORIES(
	${SRCDIR} ${SRCDIR}/${OS} ${SRCDIR}/${OS}/udp ${SUBDIR} ${SRCDIR}/${TEST}
)

ADD_EXECUTABLE(
	MQTT-SNGateway-udp
	${SRCDIR}/mainGateway.cpp
	${SRCDIR}/${OS}/udp/SensorNetwork.cpp
	${CPPSRCS} ${CSRCS}
)
TARGET_LINK_LIBRARIES(
	MQTT-SNGateway-udp
	pthread ssl crypto rt
)

# =========================== UDP6 ===========================
#INCLUDE_DIRECTORIES(
#	${SRCDIR} ${SRCDIR}/${OS} ${SRCDIR}/${OS}/udp6 ${SUBDIR} ${SRCDIR}/${TEST}
#)

#ADD_EXECUTABLE(
#	MQTT-SNGateway-udp6
#	${SRCDIR}/mainGateway.cpp
#	${SRCDIR}/${OS}/udp6/SensorNetwork.cpp
#	${CPPSRCS} ${CSRCS}
#)
#TARGET_LINK_LIBRARIES(
#	MQTT-SNGateway-udp6
#	pthread ssl crypto rt
#)

# =========================== XBEE ===========================
#INCLUDE_DIRECTORIES(
#	${SRCDIR} ${SRCDIR}/${OS} ${SRCDIR}/${OS}/xbee ${SUBDIR} ${SRCDIR}/${TEST}
#)
#ADD_EXECUTABLE(
#	MQTT-SNGateway-xbee
#	${SRCDIR}/mainGateway.cpp
#	${SRCDIR}/${OS}/xbee/SensorNetwork.cpp
#	${CPPSRCS} ${CSRCS}
#)
#TARGET_LINK_LIBRARIES(
#	MQTT-SNGateway-xbee
#	pthread ssl crypto rt
##)



install(TARGETS MQTT-SNGateway-udp RUNTIME DESTINATION "bin")
#install(TARGETS MQTT-SNGateway-udp6 RUNTIME DESTINATION "bin")
#install(TARGETS MQTT-SNGateway-xbee RUNTIME DESTINATION "bin")
