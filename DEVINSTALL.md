QIIME "Dev Install"
===================

This guide will walk you through setting everything up so you can start developing. 

The instruction steps themselves aim to be fairly generic, but the example commands are for Linux (64-bit Debian 8.1.0).

Prerequisites:

* Python 2.7
* pip
* virtualenv
* virtualenvwrapper (optional)
* A GitHub account where the QIIME project has been forked
* git

1.  Install non-Python dependencies for numpy and qiime

        sudo apt-get install libfreetype6-dev libpng12-dev pkgconf libblas-dev liblapack-dev gfortran

2.  Create and activate a Python virtual environment:

        $ virtualenv -p `which python` venv
        $ source venv/bin/activate

3.  Install NumPy and QIIME in the virtual environment

        (venv)$ pip install numpy
        (venv)$ pip install qiime

4.  Test the QIIME installation

        (venv)$ print_qiime_config.py -t

    You should get a bunch of information and the last line should contain an ```OK``` by itself.
    
5.  Get the sources: Fork on Github, clone to your machine, and make sure everything is up-to-date.

        (venv)$ mkdir ~/BIOCORE
        (venv)$ cd ~/BIOCORE
        (venv)$ git clone https://github.com/[YOUR GITHUB ACCOUNT]/qiime.git
        (venv)$ git remote add upstream https://github.com/biocore/qiime.git
        (venv)$ git pull upstream master

6.  Create a branch to run the tests

        (venv)$ git checkout -b test

7.  Run the tests:

        (venv)$ cd tests
        (venv)$ python ./all_tests.py

    It will take a while and you will see many failed tests. Don't worry. Eventually a result summary will be printed:

        .
        .
        .
    
        ==============
        Result summary
        ==============
        
        Unit test result summary
        ------------------------
        
        
        Failed the following unit tests.
        qiime/tests/test_denoiser/test_settings.py
        qiime/tests/test_parallel/test_map_reads_to_reference.py
        qiime/tests/test_workflow/test_ampliconnoise.py
        qiime/tests/test_workflow/test_pick_open_reference_otus.py
        
        Failed the following unit tests, in part or whole due to missing external applications.
        Depending on the QIIME features you plan to use, this may not be critical.
        qiime/tests/test_align_seqs.py
        qiime/tests/test_assign_taxonomy.py
        qiime/tests/test_compare_categories.py
        qiime/tests/test_denoise_wrapper.py
        qiime/tests/test_denoiser/test_denoiser.py
        qiime/tests/test_denoiser/test_flowgram_clustering.py
        qiime/tests/test_detrend.py
        qiime/tests/test_differential_abundance.py
        qiime/tests/test_exclude_seqs_by_blast.py
        qiime/tests/test_identify_chimeric_seqs.py
        qiime/tests/test_make_per_library_sff.py
        qiime/tests/test_map_reads_to_reference.py
        qiime/tests/test_normalize_table.py
        qiime/tests/test_parallel/test_assign_taxonomy.py
        qiime/tests/test_parallel/test_blast.py
        qiime/tests/test_parallel/test_identify_chimeric_seqs.py
        qiime/tests/test_parallel/test_pick_otus.py
        qiime/tests/test_pick_otus.py
        qiime/tests/test_supervised_learning.py
        qiime/tests/test_trim_sff_primers.py
        qiime/tests/test_util.py
        qiime/tests/test_workflow/test_upstream.py
        
        Script usage test result summary
        --------------------------------
        
        Ran 337 commands to test 132 scripts. 66 of these commands failed and 0 scripts could not be tested due to errors.
        Failed scripts were: align_seqs.py ampliconnoise.py assign_taxonomy.py blast_wrapper.py compare_categories.py differential_abundance.py exclude_seqs_by_blast.py identify_chimeric_seqs.py join_paired_ends.py make_phylogeny.py map_reads_to_reference.py multiple_join_paired_ends.py normalize_table.py parallel_assign_taxonomy_blast.py parallel_assign_taxonomy_rdp.py parallel_blast.py parallel_identify_chimeric_seqs.py parallel_map_reads_to_reference.py parallel_pick_otus_blast.py parallel_pick_otus_usearch61_ref.py pick_closed_reference_otus.py pick_open_reference_otus.py pick_otus.py start_parallel_jobs_torque.py supervised_learning.py

Troubleshooting failed tests
---------------

### Passing test_denoiser/test_settings.py ###

1. Install GHC

2. Build the FlowgramAlignment program included with qiime:

        (venv)$ sudo apt-get install ghc
        (venv)$ cd qiime/qiime/support_files/denoiser/FlowgramAlignment
        (venv)$ make

3. The `make` command will compile FlowAlignment and copy it to `qiime/scripts`. 

### Passing test_workflow/test_pick_open_reference_otus.py ###

### Passing test_workflow/test_ampliconnoise.py ###

### Passing test_parallel/test_map_reads_to_reference.py ###