Ansible role: perl5-modules-install
=========

Perl 5 build and install modules using cpanminus or cpm, with a deployed cpanfile

Requirements
------------

A cpanfile must be included in *files* directory (a example is included at *files/cpanfile.myproject*).

Role Variables
--------------
perl5_package_manager: you can select cpanm (default) or cpm (if you have it installed).
cpanfile: the name of the cpan file in *files* directory. Default value: cpanfile.myproject

cpanm options to run:
cpanm_configure_timeout: default value 60 (you must increase this value in large packages like Cache::Memcached)
cpanm_build_timeout: default value 3600
cpanm_test_timeout: default value 1800
cpanm_options: other options to run. Default: "-n --installdeps" (don't run tests and install dependencies)

cpm options to run:
cpm_workers: cpm can parallelize tasts running more workers. Default value 4
cpm_options: You can include other options to run with. Default value: "" (don't run tests by default)

Vars at vars/*os family*.yml
perl5_modules_build_dependencies: list of OS package names needed to build the modules included at cpanfile (example related to *cpanfile.myproject* is included for each OS family).

Dependencies
------------
This role is designed to run with **rentalia/perl5-install** role, but you can use it overriding the default values of variables, or setting:
* perl5_version: default value: "5.24.0"
* stow_dir: default value: /usr/local/stow
* perl5_real_path: default value: "{{ stow_dir }}/{{ perl5_name }}"

Example Playbook
----------------

    - hosts: servers
      roles:
        - rentalia.perl5-install
        - rentalia.perl5-modules-install
      vars:
    # vars from perl5-install
        perl5_version: "5.24.0"
        cpm_install: True
        stow_dir: /opt/stow
    # vars from perl5-modules-install
        perl5_package_manager: cpanm
        cpanm_configure_timeout: 3600
        cpm_workers: 2
        cpanfile: cpanfile
    # system packages needed (Debian!)
        perl5_modules_build_dependencies:
            - g++
            - libmemcached-dev
            - zlib1g-dev
            - libexpat1-dev
            - libxml2-dev
            - xz-utils
            - libpq-dev
            - libssl-dev
            - libcurl4-openssl-dev

License
-------
This project is licensed under the GPLv3 license - see the LICENSE file for details.

Author Information
------------------
This role was created in 2017 by [Francisco J. Tsao Santín](https://gattaca.es), devops engineer at [Rentalia](https://www.rentalia.com) 
