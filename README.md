Download Link: https://assignmentchef.com/product/solved-eth263-2210-hw-3-genome-analysis-rowclone-and-low-latency-memory
<br>
<h1>2. Genome Analysis I</h1>

2.1. Edit Distance

One of the most fundamental computational steps in most bioinformatics analyses is the detection of the differences between two DNA or protein sequences. <em>Edit distance </em>is one way to measure the differences between two genomic sequences. <em>Edit distance </em>algorithm calculates the minimum number of edit operations needed to convert one sequence into the other. Allowed edit operations are: (1) <em>substitution</em>, (2) <em>insertion</em>, and (3) <em>deletion of a character</em>. The notion of edit distance is also useful for spell checking and pattern recognition applications.

Compute the <em>Edit distance </em>for each of the following string pairs and provide the list of the edit operations (e.g., delete character ‘F’ from string “Friday” to be “riday”) used to convert the first string into the second string.

2.2. Read Mapping

During a process called read mapping in genome analysis, each genomic read is mapped onto one or more possible locations in the reference genome based on the similarity between the read and the reference genome segment at that location. Suppose that you would like to map the following reads to the human reference genome sequence.

read 1 = AAAAA_AAAAC_GGGGG read 2 = AAAAT_GGCCT_AAAAA read 3 = AAAAT_AGCGG_GCGCT read 4 = AAAAC_AAAAT_GGCCT

read 5 = AAAAA_GGCCT_AAAAC

And suppose that you need to use the following 3-step hash-based mapping method:

<ul>

 <li>Hash-based read mapper first constructs hash table to rapidly examine whether or not a short segment (called k-mers, where k is length of the segment) exists in the reference sequence.</li>

 <li>The mapper extracts 3 consecutive non-overlapping 5-mers from a read (5-mers are separated by ‘_’ sign in each read) and uses them to query the hash table. The hash table returns all the occurrence hits of each k-mer in the reference genome.</li>

 <li>For each hit, the mapper examines the differences between the entire read that includes the k-mer and the extracted reference segment (starting from the returned location of the k-mer from the hash table) using dynamic-programming edit distance function (<em>edit_distance()</em>).</li>

</ul>

The hash table (Step 1) is provided below, which includes a list of 5-mers extracted from the human reference genome and their location list (each number represents the starting location of that k-mer in the reference genome sequence). Answer the following questions:




<ul>

 <li>How many times in total the edit distance function, <em>edit_distance()</em>, will be invoked using the 3-step hash-based mapping method described above?</li>

 <li>Suppose you want to change Step 3 to include “<em>Adjacency Filtering</em>”. This means that all k-mers extracted from a read should be adjacent in the reference genome before calling the <em>edit_distance() </em>function once for that read. For example, if the first k-mer extracted from the read exists in the reference genome at location <em>x</em>, then the second k-mer and the third k-mer should also exist in the reference genome at location <em>x</em>+<em>k </em>and <em>x</em>+2<em>k</em>, respectively.</li>

</ul>

How many times in total the edit distance function, <em>edit_distance()</em>, will be invoked?

<ul>

 <li>Suppose now you want to change Step 3 to include “<em>Cheap K-mer Selection</em>”, where the threshold is 4. This means that the <em>edit_distance() </em>function will be invoked for each k-mer that occurs less frequently (less than a threshold) in the reference genome.</li>

</ul>

How many times in total the edit distance function, <em>edit_distance()</em>, will be invoked?

<h1>3. Genome Analysis II</h1>

During a process called read mapping in genome analysis, each read (i.e., genomic subsequence) is mapped to one or more locations in the reference genome based on the similarity between the read and the reference genome segment at that location. Potential mapping locations are identified based on the presence of exact short segments (i.e., <em>k-mers </em>where <em>k </em>is the length of the short segment) from the read sequence, in the reference genome. The locations of the k-mers in the reference genome are usually determined using a <em>hash table</em>. Each entry of the hash table stores a <em>key-value </em>pair, where the key is a k-mer and the value is a <em>list of locations </em>at which the k-mer occurs in the reference genome.

A challenge in designing such a hash table is deciding which k-mers to use as keys, as it affects the size of the hash table and the number of potential mapping locations, which affect the execution time of read mapping. In this question, you will be exploring the trade-offs between two strategies of k-mer selection:

<ul>

 <li>Non-overlapping 4-mers: Every <em>non-overlapping 4-mers </em>in the reference genome is used as a key in the hash table. For example, the reference AAAATTCA contains only two non-overlapping 4-mers: AAAA and TTCA. Thus, the hash table would have the following entries: {AAAA} → {1} and {TTCA} → {5}, where 1 and 5 are the start locations of the non-overlapping 4-mers (keys) in the reference.</li>

 <li>Non-overlapping 4-mer minimizers: For every non-overlapping 4-mer in the reference genome, the lexicographically minimum 4-mer of it and the two subsequent non-overlapping 4-mers is used as a key in the hash table. For example, the segment AAAATTCAACGGGCAG contains only two non-overlapping 4-mer minimizers, AAAA and ACGG. This is because AAAA is the lexicographically minimum k-mer among the first three consecutive k-mers (i.e., AAAA, TTCA, ACGG), and ACGG is the lexicographically minimum k-mer among the next three consecutive k-mers (i.e., TTCA, ACGG, and GCAG). Thus, the hash table would have the following entries: {AAAA} → {1} and {ACGG} → {9}, where 1 and 9 are the start locations of the minimizers (keys) in the reference.</li>

</ul>

Suppose that you would like to map a set of reads to the following reference genome. Note that the 4-mers are separated by ‘_’ <em>only </em>to help you identify the 4-mers easily, so you should not count them when creating a list of locations for a key.

AAAA_ATAC_TGAT_CCTT_ATAC_GTTG_TAAG_GTTT_CAAA_GTTG_ATAC_TAAG_TGAT

Answer the following questions based on the information given above:

<ul>

 <li>Please list all {key} → {value} entries in the hash table if we use all non-overlapping 4-mers as keys? The order of the entries is not important.</li>

 <li>Please list all {key} → {value} entries in the hash table if we use all non-overlapping 4-mer minimizers as keys? Please list all the entries of this hash table. The order of the entries is not important.</li>

 <li>Assume that we calculate the size of the hash table allocated in memory as: 2<sup>dlog</sup><sup>2</sup><em><sup>e</sup></em><sup>e </sup>+<em>p </em>bytes, where <em>e </em>is the total number of hash table entries and <em>p </em>is the total number of locations stored across all values. Calculate the memory footprint (in bytes) of each of the two hash tables you designed in (a) and (b). Show your work.</li>

 <li>Now assume we can query the hash table in log<sub>2 </sub><em>e </em>cycles, where <em>e </em>is the total number of entries in the hash table. A read mapper queries the hash table using the <em>first 4-mer of the read </em>and calculates the <em>edit distance </em>between the read and the reference segment at <em>each </em>location returned by the hash table. Calculating the edit distance takes <em>l</em><sup>2 </sup>cycles where <em>l </em>is the length of the read. If the edit distance between the read and a segment in the reference is higher than a certain threshold, the read mapper discards the location. We refer to cycles spent calculating the edit distance for segments at discarded locations as wasted cycles. When we profile read mapping with <em>non-overlapping 4-mers </em>and <em>nonoverlapping 4-mer minimizers </em>strategies, we find the <sup>wasted cycles</sup><sub>total cycles </sub>ratios to be 0<em>.</em>9 and 0<em>.</em>8, respectively.</li>

</ul>

Assume that we want to align the read: GTTGACCAATGA to the reference genome above. What are the <em>wasted cycles </em>when aligning the read using 1) <em>non-overlapping 4-mers </em>and 2) <em>non-overlapping 4-mer minimizers </em>strategies? Please show your work.

<ol start="4">

 <li>RowClone [150 points]</li>

</ol>

Recall that the RowClone<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a> idea presented in lecture performs the bulk copy or initialization of a page (or any arbitrary sized memory region) completely within DRAM, with support from the memory controller.

Suppose we have a cache-coherent system with six cores, a 32 KB L1 cache in each core, a 1MB private L2 cache per core, a 16MB shared L3 cache across all cores, and a single shared DRAM controller across all cores. The system supports RowClone as described in class. Physical page size is 8KB. MESI protocol is employed to keep the caches coherent.

Suppose Core 1 sends a request to the memory controller to Copy Physical Page 10 to Physical Page 12 via RowClone. Assume the two pages reside in the same DRAM subarray and the copy can be done with two consecutive ACTivate commands as a result.

<ul>

 <li>To maintain correctness of data in such a system, what should the memory controller do before performing the RowClone request? Explain step by step. Be precise. Step 1:</li>

 <li>How much data transfer is eliminated from the main memory data bus with this particular copy operation? Justify your answer and state your assumptions.</li>

</ul>

Amount of data eliminated:

<h1>5. Tiered-difficulty</h1>

Recall from your required reading on Tiered-Latency DRAM that there is a near and far segment, each containing some number of rows. Assume a very simplified memory model where there is just one bank and there are two rows in the near segment and four rows in the far segment. The time to activate and precharge a row is 25ns in the near segment and 50ns in the far segment. The time from start of activation to reading data is 10ns in the near segment and 15ns in the far segment. All other timings are negligible for this problem. Given the following memory request stream, determine the optimal assignment (minimize average latency of requests) of rows in the near and far segment (assume a fixed mapping where rows cannot migrate, a closed-row policy, and the far segment is inclusive).

time 0ns : row 0 read time 10ns : row 1 read time 100ns: row 2 read time 105ns: row 1 read time 200ns: row 3 read time 300ns: row 1 read

(a) What rows would you place in near segment? Hint: draw a timeline.

(e) Assume now that there are eight (8) rows in the near segment. Below is a plot showing the number of misses to the near segment for three applications (A, B, and C) when run alone with the specified number of rows allocated to the application in the near segment. This is similar to the plots you saw in your Utility-Based Cache Partitioning reading except for TL-DRAM instead of a cache. Determine the optimal static partitioning of the near segment when all three of these applications are run together on the system. In other words, how many rows would you allocate for each application? Hint: this should sum to eight. Optimal for this problem is defined as minimizing total misses across all applications.

(1) How many near segment rows would you allocate to A?

<h1>6. Low-Latency DRAM [150 points]</h1>

In class, we have seen the idea of Tiered-Latency DRAM (TL-DRAM). Recall that in TL-DRAM, each bitline of a subarray is segmented into two portions by adding isolation transistors in between, creating two segments on the bitline: the <em>near segment </em>and the <em>far segment</em>. The near segment is close to the sense amplifiers whereas the far segment is far away.

(a) Why is accessing a row in the near segment faster in TL-DRAM compared to a commodity DRAM chip?

Now, assume that:

<ul>

 <li>We have a system that uses the near segment as a cache to the far segment, and the far segment contains main memory locations.</li>

 <li>The near segment is not visible to software and the rows that are cached in it are completely managed by the memory controller.</li>

 <li>The far segment is inclusive of the near segment.</li>

 <li>In each subarray, the far segment contains 496 rows whereas the total number of rows in the subarray is 512.</li>

 <li>What is the capacity loss in main memory size when we use TL-DRAM as opposed to commodity DRAM with the same number of total DRAM rows? Express this as a fraction of total memory</li>

 <li>What is the tag store size that needs to be maintained on a per subarray basis in the DRAM controller if the near segment is used as a fully-associative write-back cache? Assume the replacement algorithm is <em>Most Recently Used </em>(MRU) and use the minimum number of bits possible to implement the replacement algorithm. Show your work.</li>

</ul>

Now assume near segment and far segment are <em>exclusive</em>. In other words, both contain memory rows, and a memory row can only be in one of the segments. When a memory row in the far segment is referenced, it is brought into the near segment by exchanging the MRU row in the near segment with the row in the far segment. Note that a row can end up in a <em>different </em>location in the far segment after being moved to the near segment and then back to the far segment.

<ul>

 <li>When the near segment is used as an <em>exclusive </em>cache to the far segment, what is the capacity loss in main memory size when we use TL-DRAM as opposed to commodity DRAM with the same number of total DRAM rows? Express this as a fraction of total memory capacity lost (no need to simplify the fraction).</li>

 <li>What is the tag store size that needs to be maintained on a per subarray basis in the DRAM controller if the near segment is used as an <em>exclusive </em>fully-associative write-back cache? Assume the replacement algorithm is MRU and use the minimum number of bits possible to implement the replacement algorithm. Show your work.</li>

 <li>Assume the near and far segments are visible to the operating system (OS) and the OS can allocate physical pages in either of the segments. Answer the following questions as <em>True </em>or <em>False </em>and provide explanation to your answer.

  <ul>

   <li>The OS <em>cannot </em>manage the near segment as a cache at granularity smaller than the physical page granularity.</li>

   <li>If the near segment is used as an OS-managed cache to store frequently-accessed pages, the cache can <em>only </em>be exclusive.</li>

   <li>An OS-managed near segment cache does <em>not </em>incur <em>any </em>tag store overhead in the memory controller.</li>

  </ul></li>

</ul>

<a href="#_ftnref1" name="_ftn1">[1]</a> Seshadri et al., <em>“RowClone: Fast and Energy-Efficient In-DRAM Bulk Data Copy and Initialization.” </em>In Proceedings of the 46th International Symposium on Microarchitecture (MICRO), 2013.