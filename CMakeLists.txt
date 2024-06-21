project(theora)

# find out the architecture and set our included assembly define
if(UNREAL_IS_X86)
	add_definitions(-DOC_X86_ASM)
elseif(UNREAL_IS_AMD64)
	add_definitions(-DOC_X86_64_ASM)
elseif(UNREAL_IS_ARM)
	add_definitions(-DOC_ARM_ASM)
endif()

# libtheora code
file(GLOB THEORA_INCLUDES include/theora/*.h)
file(GLOB THEORA_SOURCES lib/*.c)
source_group("Inc" FILES ${THEORA_INCLUDES})
source_group("Src" FILES ${THEORA_SOURCES})

if (MSVC AND (UNREAL_IS_X86 OR UNREAL_IS_AMD64))
	file(GLOB THEORA_ASM_INCLUDES lib/x86_vc/*.h)
	file(GLOB THEORA_ASM_SOURCES lib/x86_vc/*.c)
elseif(UNREAL_IS_X86 OR UNREAL_IS_AMD64)
	file(GLOB THEORA_ASM_INCLUDES lib/x86/*.h)
	file(GLOB THEORA_ASM_SOURCES lib/x86/*.c)
elseif(UNREAL_IS_ARM)
	file(GLOB THEORA_ASM_INCLUDES lib/arm/*.h)
	file(GLOB THEORA_ASM_SOURCES lib/arm/*.c)
endif()
source_group("Inc\\asm" FILES ${THEORA_ASM_INCLUDES})
source_group("Src\\asm" FILES ${THEORA_ASM_SOURCES})

# set include directories
include_directories(
	../ogg/include
	./include
)

# build libtheora static library
add_library(theora STATIC ${THEORA_INCLUDES} ${THEORA_SOURCES} ${THEORA_ASM_INCLUDES} ${THEORA_ASM_SOURCES})

