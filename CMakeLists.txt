cmake_minimum_required(VERSION 3.11...3.20)
project(ziplib)

set (CMAKE_CXX_STANDARD 11)
add_definitions(-std=c++11)

add_subdirectory(extlibs/bzip2)
add_subdirectory(extlibs/lzma)
add_subdirectory(extlibs/zlib)

set(headers_compression
	compression/compression_interface.h
)

set(headers_compression_bzip2
	compression/bzip2/bzip2_decoder.h
	compression/bzip2/bzip2_decoder_properties.h
	compression/bzip2/bzip2_encoder.h
	compression/bzip2/bzip2_encoder_properties.h
)

set(headers_compression_deflate
	compression/deflate/deflate_decoder.h
	compression/deflate/deflate_decoder_properties.h
	compression/deflate/deflate_encoder.h
	compression/deflate/deflate_encoder_properties.h
)

set(headers_compression_lzma
	compression/lzma/lzma_decoder.h
	compression/lzma/lzma_decoder_properties.h
	compression/lzma/lzma_encoder.h
	compression/lzma/lzma_encoder_properties.h
)

set(headers_compression_lzma_detail
	compression/lzma/detail/lzma_alloc.h
	compression/lzma/detail/lzma_handle.h
	compression/lzma/detail/lzma_header.h
	compression/lzma/detail/lzma_in_stream.h
	compression/lzma/detail/lzma_out_stream.h
)

set(headers_compression_store
	compression/store/store_decoder.h
	compression/store/store_decoder_properties.h
	compression/store/store_encoder.h
	compression/store/store_encoder_properties.h
)

set(headers_detail
	detail/EndOfCentralDirectoryBlock.h
	detail/ZipCentralDirectoryFileHeader.h
	detail/ZipGenericExtraField.h
	detail/ZipLocalFileHeader.h
)

set(headers_methods
	methods/Bzip2Method.h
	methods/DeflateMethod.h
	methods/ICompressionMethod.h
	methods/LzmaMethod.h
	methods/StoreMethod.h
	methods/ZipMethodResolver.h
)

set(headers_streams
	streams/compression_decoder_stream.h
	streams/compression_encoder_stream.h
	streams/crc32stream.h
	streams/memstream.h
	streams/nullstream.h
	streams/serialization.h
	streams/substream.h
	streams/teestream.h
	streams/zip_cryptostream.h
)

set(headers_streams_streambuffs
	streams/streambuffs/compression_decoder_streambuf.h
	streams/streambuffs/compression_encoder_streambuf.h
	streams/streambuffs/crc32_streambuf.h
	streams/streambuffs/mem_streambuf.h
	streams/streambuffs/null_streambuf.h
	streams/streambuffs/sub_streambuf.h
	streams/streambuffs/tee_streambuff.h
	streams/streambuffs/zip_crypto_streambuf.h
)

set(headers_utils
	utils/enum_utils.h
	utils/stream_utils.h
	utils/time_utils.h
)

set(headers_root
	ZipArchive.h
	ZipArchiveEntry.h
	ZipFile.h
)


set(src_detail
	detail/EndOfCentralDirectoryBlock.cpp
	detail/ZipCentralDirectoryFileHeader.cpp
	detail/ZipGenericExtraField.cpp
	detail/ZipLocalFileHeader.cpp
)

set(src_root
	ZipArchive.cpp
	ZipArchiveEntry.cpp
	ZipFile.cpp
)

set(headers_all
	${headers_compression_bzip2}
	${headers_compression_deflate}
	${headers_compression_lzma}
	${headers_compression_lzma_detail}
	${headers_compression_store}
	${headers_detail}
	${headers_methods}
	${headers_streams}
	${headers_streams_streambuffs}
	${headers_utils}
	${headers_root}
)

set(src_all
	${src_detail}
	${src_root}
)

set(sources
	${headers_all}
	${src_all}
)

source_group(headers\\compression					FILES ${headers_compression})
source_group(headers\\compression\\bzip2			FILES ${headers_compression_bzip2})
source_group(headers\\compression\\deflate			FILES ${headers_compression_deflate})
source_group(headers\\compression\\lzma				FILES ${headers_compression_lzma})
source_group(headers\\compression\\lzma_detail		FILES ${headers_compression_lzma_detail})
source_group(headers\\compression\\store			FILES ${headers_compression_store})
source_group(headers\\detail						FILES ${headers_detail})
source_group(headers\\methods						FILES ${headers_methods})
source_group(headers\\streams						FILES ${headers_streams})
source_group(headers\\streams\\streambuffs			FILES ${headers_streams_streambuffs})
source_group(headers\\utils							FILES ${headers_utils})
source_group(headers								FILES ${headers_root})
source_group(sources\\detail						FILES ${src_detail})
source_group(sources								FILES ${src_root})

add_library(${PROJECT_NAME} STATIC ${sources})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_FLAGS " /wd4996")
target_link_libraries(${PROJECT_NAME} bzip2 lzma zlib)
