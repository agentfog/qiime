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

1.  Install non-Python dependencies for numpy and qiime:

        sudo apt-get install libfreetype6-dev libpng12-dev pkgconf libblas-dev liblapack-dev gfortran

2.  Create and activate a Python virtual environment:

        $ virtualenv -p `which python` venv
        $ source venv/bin/activate

3.  Install NumPy and QIIME in the virtual environment:

        (venv)$ pip install numpy
        (venv)$ pip install qiime

4.  Test the QIIME installation:

        (venv)$ print_qiime_config.py -t

    You should get a bunch of information and the last line should contain an ```OK``` by itself.
    
5.  Fork the QIIME project on Github, clone to your machine, and make sure everything is up-to-date:

        (venv)$ mkdir ~/BIOCORE
        (venv)$ cd ~/BIOCORE
        (venv)$ git clone https://github.com/[YOUR GITHUB ACCOUNT]/qiime.git
        (venv)$ git remote add upstream https://github.com/biocore/qiime.git
        (venv)$ git pull upstream master

6.  Create a branch to run the tests:

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

1. Install GHC.

        (venv)$ sudo apt-get install ghc

2. Build the FlowgramAlignment program included with qiime:

        (venv)$ cd ~/BIOCORE/qiime/qiime/support_files/denoiser/FlowgramAlignment
        (venv)$ make

3. The `make` command will compile FlowAlignment and copy it to `qiime/scripts`. You must add that directory to your PATH. If you are using virtualenv, this is done in the ```activate``` script.

### Passing test_workflow/test_pick_open_reference_otus.py ###

1. Make sure Java works. OpenJDK will do fine.

2. Download rdp_classifier-2.2 from [SourceForge](http://sourceforge.net/projects/rdp-classifier/files/rdp-classifier/rdp_classifier_2.2.zip/download) and unzip it:

	(venv)$ cd ~/BIOCORE
        (venv)$ unzip rdp_classifier-2.2.zip

3. Set ```RDP_JAR_PATH``` to point to ```rdp_classifier-2.2.jar```:

        export RDP_JAR_PATH=$HOME/BIOCORE/rdp_classifier_2.2/rdp_classifier-2.2.jar

If using virtualenv, you should add the line above to your ```activate``` script below ```export PATH```.

4. Get USEARCH 6.1.544 (beta) from [Drive5](http://www.drive5.com/usearch/download.html).

5. Make sure the executable is named ```usearch61``` and has execute permission:

        (venv)$ mv usearch6.1.544_i86linux32 usearch61
        (venv)$ chmod +x usearch61

6. Add the directory containing ```usearch61``` to your PATH.

### Passing test_workflow/test_ampliconnoise.py ###

1. Install AmpliconNoise dependencies:

        (venv)$ sudo apt-get install mpi-default-dev
        (venv)$ sudo apt-get install mpi-default-bin
        (venv)$ sudo apt-get install libgsl0-dev

2. Download [AmpliconNoise](http://ampliconnoise.googlecode.com/files/AmpliconNoiseV1.27.tar.gz).

3. Install AmpliconNoise 1.27:

        (venv)$ tar xvzf AmpliconNoiseV1.27.tar.gz
        (venv)$ cd AmpliconNoiseV1.27
        (venv)$ make
        (venv)$ make install

4. Export the necessary environment variables. If using virtualenv, add the following lines to your ```activate``` script:

        export PATH=$HOME/BIOCORE/AmpliconNoiseV1.27/Scripts:$HOME/BIOCORE/AmpliconNoiseV1.27/bin:$PATH
        export PYRO_LOOKUP_FILE=$HOME/BIOCORE/AmpliconNoiseV1.27/Data/LookUp_E123.dat
        export SEQ_LOOKUP_FILE=$HOME/BIOCORE/AmpliconNoiseV1.27/Data/Tran.dat"

### Passing test_parallel/test_map_reads_to_reference.py ###

1. Install [BLAT](http://hgdownload.cse.ucsc.edu/admin/exe/userApps.src.tgz)

2. Install [BWA](http://sourceforge.net/projects/bio-bwa/files/bwa-0.7.12.tar.bz2/download)

3. Add BLAT and BWA to PATH

        export PATH=$PATH:$HOME/BIOCORE/kentUtils:$HOME/BIOCORE/bwa-07.12

4. Get USEARCH 5.2.236 from [Drive5](http://www.drive5.com/usearch/download.html). 

5. Make sure the executable is named ```usearch``` and has execute permission:

        (venv)$ mv usearch5.2.236_i86linux32 usearch
        (venv)$ chmod +x usearch

6. Add the directory containing ```usearch``` to your PATH.