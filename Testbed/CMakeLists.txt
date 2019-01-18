# Some flags for Freeglut and GLUI.
add_definitions( -DFREEGLUT_EXPORTS -DFREEGLUT_STATIC -D_CRT_SECURE_NO_WARNINGS )

# Define the framework files.
set(Testbed_Framework_SRCS
        Framework/DebugDraw.cpp
        Framework/DebugDraw.h
        Framework/Main.cpp
        Framework/Test.cpp
        Framework/Test.h
        )

#define the test files.
set(Testbed_Tests_SRCS
        Tests/TestEntries.cpp
        Tests/AddPair.h
        Tests/ApplyForce.h
        Tests/BasicSliderCrank.h
        Tests/BodyTypes.h
        Tests/Breakable.h
        Tests/Bridge.h
        Tests/Cantilever.h
        Tests/Car.h
        Tests/Chain.h
        Tests/chainProblem.h
        Tests/CharacterCollision.h
        Tests/CollisionFiltering.h
        Tests/CollisionProcessing.h
        Tests/CompoundShapes.h
        Tests/Confined.h
        Tests/ContinuousTest.h
        Tests/ConvexHull.h
        Tests/ConveyorBelt.h
        Tests/DistanceTest.h
        Tests/Dominos.h
        Tests/DumpShell.h
        Tests/DynamicTreeTest.h
        Tests/EdgeShapes.h
        Tests/Gears.h
        Tests/HeavyOnLight.h
        Tests/HeavyOnLightTwo.h
        Tests/Mobile.h
        Tests/MobileBalanced.h
        Tests/MotorJoint.h
        Tests/OneSidedPlatform.h
        Tests/Pinball.h
        Tests/PolyCollision.h
        Tests/PolyShapes.h
        Tests/Prismatic.h
        Tests/Pulleys.h
        Tests/Pyramid.h
        Tests/RayCast.h
        Tests/Revolute.h
        Tests/Rope.h
        Tests/RopeJoint.h
        Tests/SensorTest.h
        Tests/ShapeEditing.h
        Tests/Skier.h
        Tests/SliderCrank.h
        Tests/SphereStack.h
        Tests/TheoJansen.h
        Tests/TimeOfImpact.h
        Tests/Tiles.h
        Tests/Tumbler.h
        Tests/VaryingFriction.h
        Tests/VaryingRestitution.h
        Tests/VerticalStack.h
        Tests/Web.h
        )


set(Testbed_Base
        glfw/internal.h
        glfw/glfw_config.h
        glfw/glfw3.h
        glfw/glfw3native.h
        glfw/context.c
        glfw/init.c
        glfw/input.c
        glfw/monitor.c
        glfw/vulkan.c
        glfw/window.c
        imgui/imconfig.h
        imgui/imgui.cpp
        imgui/imgui_demo.cpp
        imgui/imgui_draw.cpp
        imgui/imgui_impl_glfw_gl3.cpp
        imgui/imgui_impl_glfw_gl3.h
        imgui/imgui_internal.h
        imgui/stb_rect_pack.h
        imgui/stb_textedit.h
        imgui/stb_truetype.h
        Data/DroidSans.ttf
        )

set(Testbed_Tests_LINUX
         glad/glad.c
         glad/glad.h
         glad/khrplatform.h
         glfw/x11_platform.h
         glfw/xkb_unicode.h
         glfw/linux_joystick.h
         glfw/posix_time.h
         glfw/posix_tls.h
         glfw/glx_context.h
         glfw/egl_context.h
         glfw/x11_init.c
         glfw/x11_monitor.c
         glfw/x11_window.c
         glfw/glx_context.h
         glfw/glx_context.c
         glfw/glext.h
         glfw/xkb_unicode.c
         glfw/linux_joystick.c
         glfw/posix_time.c
         glfw/posix_tls.c
         glfw/glx_context.c
         glfw/egl_context.c
    )



# These are used to create visual studio folders.
source_group(Framework FILES ${Testbed_Framework_SRCS})
source_group(Tests FILES ${Testbed_Tests_SRCS})
source_group(Glfw FILES ${Testbed_Base})
source_group(Linux FILES ${Testbed_Tests_LINUX})


include_directories (
        ${OPENGL_INCLUDE_DIR}
        ${Box2D_SOURCE_DIR}
        ${CMAKE_CURRENT_SOURCE_DIR}
)

if(APPLE)
    # We are not using the Apple's framework version, but X11's
    include_directories( /usr/X11/include )
    link_directories( /usr/X11/lib )
    set (OPENGL_LIBRARIES GL GLU GLUT X11)
endif(APPLE)

find_package(OpenGL REQUIRED)
if(NOT OPENGL_FOUND)
    message("ERROR: OpenGL not found")
endif(NOT OPENGL_FOUND)
set(OPENGL_LIBRARIES GL GLU X11)

add_executable(Testbed
        ${Testbed_Framework_SRCS}
        ${Testbed_Tests_SRCS}
        ${Testbed_Base}
        ${Testbed_Tests_LINUX}
        )

target_link_libraries (
        Testbed
        ${CMAKE_DL_LIBS}
        Box2D
        Xrandr
        Xinerama
        Xcursor
        ${OPENGL_LIBRARIES} -lpthread -lm -lz
)