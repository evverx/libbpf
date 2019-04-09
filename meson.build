# SPDX-License-Identifier: LGPL-2.1 OR BSD-2-Clause
project('libbpf', 'c',
        version : '0.0.2',
        license : 'LGPL-2.1 OR BSD-2-Clause')

libbpf_src_dir = './'

libbpf_sources = files(run_command('find',
        ['@0@/src'.format(libbpf_src_dir),
        '-type',
        'f',
        '-name',
        '*.h',
        '-o',
        '-name',
        '*.c']).stdout().split())

libbpf_headers = files(
        join_paths(libbpf_src_dir, 'src/bpf.h'),
        join_paths(libbpf_src_dir, 'src/btf.h'),
        join_paths(libbpf_src_dir, 'src/libbpf.h'))


feature_rellocarray = run_command(join_paths(libbpf_src_dir, 'scripts/check-reallocarray.sh'))

libbpf_c_args = [
              '-g',
              '-O2',
              '-Werror',
              '-Wall',
              '-Wno-pointer-arith',
              '-Wno-sign-compare'
]

if feature_rellocarray.stdout().strip() != ''
        libbpf_c_args += '-DCOMPAT_NEED_REALLOCARRAY'
endif

libbpf_includes = include_directories(
        join_paths(libbpf_src_dir, 'include'),
        join_paths(libbpf_src_dir, 'include/uapi'))


# bpf_includes are required to include bpf.h, btf.h, libbpf.h
bpf_includes = include_directories(
        join_paths(libbpf_src_dir, 'src'))

libbpf_static = static_library(
        'bpf',
        libbpf_sources,
        include_directories : libbpf_includes,
        install : false,
        c_args : libbpf_c_args)

libbpf_static_dep = declare_dependency(link_with : libbpf_static)

libbpf_map = join_paths(libbpf_src_dir, 'src/libbpf.map')

libbpf_c_args += ['-fPIC', '-fvisibility=hidden']

libbpf_shared = shared_library(
        'bpf',
        include_directories : libbpf_includes,
        link_depends : libbpf_map,
        install : false,
        c_args : libbpf_c_args)

libbpf_shared_dep = declare_dependency(link_with : libbpf_shared)
