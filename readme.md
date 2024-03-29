[<img src="img/khammix.png"/>]()

# Kernelized quantitative trait HAplotype-based Mapping using MIXed models (KHAMMIX) 

## Objective, remarks and instructions :

### 🎯 Objective

* The KHAMMIX software is designed to conduct a genome-wide scan for identifying quantitative trait loci (QTLs) and mapping them, utilizing a sliding window approach with $L > 1$ SNP markers along phased chromosomes. Consequently, each window encompasses $n_h$ haplotypes, where $1< n_h \leq 2^L$, under the assumption of no monomorphic SNP markers. The central position of the sliding window is considered as the potential location harboring the putative QTL during the genome scan.  

* At the central position of the sliding window, KHAMMIX evaluates and tests various mixed models and hypotheses, inspired by the works of Jacquin $\textit{et al.}$ (2014) and Lafarge $\textit{et al.}$ (2017). Particularly, the former study advocates the utilization of identity-by-state (IBS) of haplotypes for such analyses.  The software evaluates the following models and hypotheses for each window :
$$\\
\left\{
    \begin{array}{ll}
      Y = X\beta + Z_hh+ Zu + \varepsilon \hspace{0.2cm} (H1) \\
      Y = X\beta + Zu + \varepsilon \hspace{0.2cm} (H0)
    \end{array}
\right.$$
where :
  * $Y$ represents the vector of $n$ measured phenotypes for the analyzed trait.
  * $\beta$ denotes the vector of fixed effects.
  * $h$ represents the vector of random effects of haplotypes following a multivariate normal distribution, $i.e. h \sim \mathcal{N}_{n_h}(0,H_{\sigma^2_{h}})$, where $H$ is the covariance matrix describing the identity-by-state (IBS) status between haplotypes.
  * $u$ denotes the vector of random polygenic effects following a multivariate normal distribution, $i.e. u \sim \mathcal{N}_{n}(0,K_{\sigma^2_{K}})$, where $K$ is the genomic covariance matrix also known as the Gram matrix. $K$ can be constructed using either the VanRaden (2008) method which represents a linear additive kernel, or a Gaussian kernel which is a non-linear universal approximator capable of modeling epistatic effects (Jacquin $\textit{et al.}$, 2016).
  * $X$, $Z_h$, and $Z$ represent the design matrices that relate fixed and random effects to the measured phenotypes.
  * $\varepsilon$ denotes the vector of residuals.
  
### 🔍 Remarks 

* The KHAMMIX software can only operate in a Unix/Linux environment, provided that R and its associated libraries are installed beforehand

* Parallelization of computations launched by this software can only be achieved through a computing cluster and not on a personal computer

* Sequential computation can also be performed on a personal computer, in a Unix/Linux environment, provided that R and its associated libraries are installed, but it is not recommended due to the long computation time

* Sequential computation can be very time-consuming (one to several days) and is therefore not recommended if a computing cluster is available

* On a computing cluster, the computation time for a complete genome scan (i.e., scanning all chromosomes by parallel computation) can vary from a few hours (2 to 3 hours on average) to a maximum of one day depending on the cluster's usage by other users (number of jobs/priorities, etc.)

### 💻 Instructions

* Copy the "khammix" folder to the current user's directory on a computing cluster or personal computer.

* Within the "khammix" folder, execute the command chmod u+rwx make_scripts_programs_executable.sh, followed by ./make_scripts_programs_executable.sh.

* Replace the following five files: genotypes.txt, incidence_fixed_effects.txt, phased_genotypes.txt, phenotypes.txt, physical_map.txt in the "data_parameters" folder.

    ⚠️ Ensure that the new data files replacing the old ones adhere to the same format.

* Prior to initiating an analysis, execute the command ./clean_scans.sh within the "khammix" folder to clear any existing directories of the type "genome_scan_chromo_num_".

    ⚠️ It is crucial to avoid running the command ./clean_scans.sh after completing a full genome analysis, as it will permanently erase all results.

* Open the main script by typing gedit khammix.sh & (or nedit khammix.sh & if gedit is not installed), or alternatively nano khammix.sh to modify the file. At the beginning of the script, modify the five parameters: trait_name, nb_snp_hap, nb_chromosomes, kernel_index, and local_or_cluster according to the desired analysis specifications. trait_name corresponds to the trait under analysis, while nb_snp_hap denotes the size of the sliding window (i.e., haplotypes) in terms of the number of markers.

* To start the genome scan for the analyzed trait, simply execute the command ./khammix.sh. Note that the script's outputs can be redirected using the command ./khammix.sh > khammix_outputs.

* To monitor the progress of parallel jobs, use the command qstat -u username.

* Upon completion of the jobs, execute the command ./get_results_scans.sh to retrieve all results, including RLRT graphs per chromosome, significant regions/windows with corresponding haplotypes per chromosome, etc.

* Final note: The "khammix" folder can be duplicated multiple times, with each copy renamed, to analyze multiple traits in parallel. For instance, folders like "khammix_LLGTH", "khammix_NBL", "khammix_TIL", etc., can be created for example from the provided data set.

## References :

* Lafarge T., Bueno C., Frouin J., Jacquin L., Courtois B., Ahmadi N. (2017). Genome-wide association analysis for heat tolerance at flowering
detected a large set of genes involvedin adaptation to thermal and other stresses

* Jacquin et al. (2014). Using haplotypes for the prediction of allelic identity to fine- map QTL : characterization and properties

* Jacquin et al. (2016). A unified and comprehensible view of parametric and kernel methods for genomic prediction with application to rice

* VanRaden, P.M. (2008) Efficient Methods to Compute Genomic Predictions. Journal of Dairy Science, 91:4414-4423 
