cmake_minimum_required(VERSION 3.17)
project(minecraft)

set(CMAKE_CXX_STANDARD 20)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}")

add_executable(${CMAKE_PROJECT_NAME}
        # game:
        Source.cpp
        Minecraft.cpp
        Minecraft.h
        Player.cpp
        Player.h
        Client.cpp
        Client.h
        Server.cpp
        Server.h
        Cube.cpp
        Cube.h
        PlayerController.cpp
        PlayerController.h
        Map.cpp
        Map.h
        MinecraftMsgType.cpp
        MinecraftMsgType.h
        MinecraftConsts.h
        # 3d engine:
        engine/Consts.h
        engine/Vec4D.h
        engine/Vec4D.cpp
        engine/Vec3D.cpp
        engine/Vec3D.h
        engine/Vec2D.cpp
        engine/Vec2D.h
        engine/Matrix4x4.h
        engine/Matrix4x4.cpp
        engine/Triangle.h
        engine/Triangle.cpp
        engine/Plane.h
        engine/Plane.cpp
        engine/Mesh.h
        engine/Mesh.cpp
        engine/utils/Log.h
        engine/utils/Log.cpp
        engine/utils/Time.h
        engine/utils/Time.cpp
        engine/utils/Timer.cpp
        engine/utils/Timer.h
        engine/ResourceManager.h
        engine/ResourceManager.cpp
        engine/World.h
        engine/World.cpp
        engine/Camera.h
        engine/Camera.cpp
        engine/Screen.h
        engine/Screen.cpp
        engine/Engine.h
        engine/Engine.cpp
        engine/Keyboard.cpp
        engine/Keyboard.h
        engine/Mouse.cpp
        engine/Mouse.h
        engine/SoundController.cpp
        engine/SoundController.h
        engine/ObjectController.cpp
        engine/ObjectController.h
        engine/animation/Animation.h
        engine/animation/Timeline.cpp
        engine/animation/Timeline.h
        engine/animation/Interpolation.h
        engine/animation/Animation.cpp
        engine/animation/ATranslate.h
        engine/animation/AScale.h
        engine/animation/ARotate.h
        engine/animation/AWait.h
        engine/animation/AFunction.h
        engine/animation/AAttractToPoint.h
        engine/animation/ARotateRelativePoint.h
        engine/animation/ARotateLeft.h
        engine/animation/Interpolation.cpp
        engine/animation/Animations.h
        engine/animation/AShowCreation.h
        engine/animation/AShowUncreation.h
        engine/physics/RigidBody.cpp
        engine/physics/RigidBody.h
        engine/physics/Simplex.h
        engine/physics/HitBox.cpp
        engine/physics/HitBox.h
        engine/Object.cpp
        engine/Object.h
        engine/gui/Button.cpp
        engine/gui/Button.h
        engine/gui/Window.cpp
        engine/gui/Window.h
        engine/network/ClientUDP.cpp
        engine/network/ClientUDP.h
        engine/network/MsgType.cpp
        engine/network/MsgType.h
        engine/network/ReliableMsg.cpp
        engine/network/ReliableMsg.h
        engine/network/ServerUDP.cpp
        engine/network/ServerUDP.h
        engine/network/UDPConnection.cpp
        engine/network/UDPConnection.h
        engine/network/UDPSocket.cpp
        engine/network/UDPSocket.h
        )

if(APPLE OR UNIX)
    include_directories(/usr/local/include)
else()
    set(SFML_DIR "C:/Libraries/SFML/lib/cmake/SFML")
    set(SFML_STATIC_LIBRARIES TRUE)
endif()

find_package(SFML 2.5.1 COMPONENTS graphics audio REQUIRED)

if (SFML_FOUND)
    include_directories(${SFML_INCLUDE_DIR})
endif()

target_link_libraries(${CMAKE_PROJECT_NAME} sfml-audio sfml-network sfml-graphics sfml-window sfml-system)

# OpenGL part
if (APPLE)
    set(GLEW_H /usr/local/Cellar/glew/2.1.0/include/GL)
    set(GLFW_H /usr/local/Cellar/glfw/3.2.1/include/GLFW)
    include_directories(${GLEW_H} ${GLFW_H})

    set(GLEW_LINK /usr/local/Cellar/glew/2.1.0/lib/libGLEW.2.1.dylib)
    set(GLFW_LINK /usr/local/Cellar/glfw/3.2.1/lib/libglfw.3.dylib)
    link_libraries(${OPENGL} ${GLEW_LINK} ${GLFW_LINK})

    target_link_libraries(${CMAKE_PROJECT_NAME} "-framework OpenGL")
    target_link_libraries(${CMAKE_PROJECT_NAME} "-framework GLUT")
elseif(UNIX)
    find_package(OpenGL REQUIRED)
    find_package(GLUT REQUIRED)

    target_link_libraries(${CMAKE_PROJECT_NAME} ${OPENGL_LIBRARIES})
    target_link_libraries(${CMAKE_PROJECT_NAME} ${GLUT_LIBRARY})
endif()