# Sunbeam Kraken Uniq (simple use) extension 

This is a template to use to extend the [Sunbeam pipeline](https://github.com/sunbeam-labs/sunbeam) with the [Kraken Uniq](https://github.com/fbreitwieser/krakenuniq), a tool for classifying metagenomic data with unique k-mer counting for more specific results. There are three major parts to a Sunbeam extension: 

 - `sbx_kraken_uniq_env.yml` specifies the extension's dependencies
 - `config.yml` contains configuration options that can be specified by the user when running an extension
 - `sbx_kraken_uniq.rules` contains the rules (logic/commands run) of the extension
 
## Anatomy of the extension

The dependencies required for this extension are listed in the `sbx_kraken_uniq_env.yml` file.

The `config.yml` contains parameters (threads and database filepath) that the user can modify when running an extension. Default values should be specified for each bottom-level key.

Finally, `sbx_kraken_uniq.rules` contains the actual logic for the extension, including required input and output files. Check out [the Snakemake tutorial](http://snakemake.readthedocs.io/en/stable/tutorial/basics.html) and any of the [extensions by sunbeam-labs](https://github.com/sunbeam-labs) for information on how to modify the rules.

Currently the extension only works on PAIRED END READS.

## Installing the extension

To install (see legacy instructions below):

    sunbeam extend https://github.com/ArwaAbbas/sbx_kraken_uniq

Configuration options for `sbx_kraken_uniq` are automatically added on `sunbeam init`. If you're installing an extension in a project where you already have a config file, run the following to add the options for your newly added extension to your config (the `-i` flag means in-place config file modification; remove the `-i` flag to see the new config in stdout):

    sunbeam config update -i sunbeam_config.yml

This extension has dependencies that conflict with those of [kraken](http://ccb.jhu.edu/software/kraken/) which is used in the regular Sunbeam pipeline. Therefore, it is run with in its own Conda environment which is specified when it is run (see below).

This extension **DOES NOT** download and build the Kraken Uniq database itself. It can be used with an existing Kraken database, or the user can build ones per instructions in the Kraken Uniq manual.

## Running sbx_kraken_uniq

To run, simply run Sunbeam as usual with the target rule specified:
By default, it does not write classified and unclassified reads to separate FASTA files. This output can be added by adjusting the rules if desired.

    sunbeam run --configfile sunbeam_config.yml --use-conda all_kraken_uniq

The `--use-conda` flag is required to let Snakemake know that you want to use the conda environment(s) included with your extension.

-------------

## Legacy install instructions for sunbeam <3.0

Installing an extension is as simple as cloning (or moving) your extension directory into the sunbeam/extensions/ folder, installing requirements through Conda, and adding the new options to your existing configuration file: 

    source activate sunbeam 
    git clone https://github.com/ArwaAbbas/sbx_kraken_uniq/ sunbeam/extensions/sbx_kraken_uniq
    cat sunbeam/extensions/sbx_kraken_uniq/config.yml >> sunbeam_config.yml

