cmake_minimum_required(VERSION 3.29)
project(rnnoise_cmakeprj)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_C_FLAGS "-pedantic -Wall -Wextra -Wno-sign-compare -Wno-parentheses -Wno-long-long -fPIC -DHAVE_CONFIG_H")

include_directories(./src ./include ./)
LINK_LIBRARIES(m)
add_library(rnnoise_library SHARED
        src/celt_lpc.c
        src/kiss_fft.c
        src/nnet.c
        src/pitch.c
        src/rnnoise_data.c
        src/denoise.c
        src/nnet_default.c
        src/parse_lpcnet_weights.c
        src/rnn.c
        src/rnnoise_tables.c
)

add_executable(dump_feature
        src/denoise.c
        src/pitch.c
        src/celt_lpc.c
        src/kiss_fft.c
        src/parse_lpcnet_weights.c
        src/rnnoise_tables.c
        src/dump_features.c
)

target_compile_definitions(dump_feature PUBLIC -DTRAINING)

# Add an executable with the above sources
add_executable(demo
        main.c
)

# link the rnnoise library target with the demo target
target_link_libraries(demo
        rnnoise_library
)

add_executable(dump_weights_blob
        src/write_weights.c
)
target_link_libraries(dump_weights_blob
        rnnoise_library
)