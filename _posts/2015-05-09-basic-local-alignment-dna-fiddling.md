---
title: 'Basic Local Alignment &#8211; DNA fiddling'
author: Koen Hufkens
layout: post
permalink: /2015/05/09/basic-local-alignment-dna-fiddling/
categories:
  - Science
  - Software
tags:
  - DNA
  - science
  - software
---
My flatmate was doing some manual queries of DNA sequences using the <a href="http://www.ncbi.nlm.nih.gov/">National Center for Biotechnology Information (NCBI)</a> online <a href="http://blast.ncbi.nlm.nih.gov/Blast.cgi">Basic Local Alignment Search Tool (BLAST)</a>. Doing this manual seemed like a waste of time. So, I looked around a bit to find an optimal automated solution. After trying several R and Perl solutions, nothing seemed stable enough to be of any use. In the end I settled on <a href="http://biopython.org/wiki/Main_Page">Biopython</a> and their online query routine in their Blast class of tools. Of all tools this seemed the most mature and especially the most stable for querying the NCBI server.

The basic code is listed below, spitting out the first match on a query. However, I must say this was more challenging than I imagined. I think the lesson I learned from this is that they discourage using the online tool (by making it rather hard to use) in order to move as much traffic offline. So if you ever need to do this, just mirror the whole BLAST database or the parts you need and do a local search. This will save you a lot of time.

<pre class="lang:python decode:true">import sys
from Bio import SeqIO
from Bio.Blast import NCBIWWW,NCBIXML

# get filename from command line
fasta_file = sys.argv[1]

# read fasta file
fasta_records = list(SeqIO.parse(fasta_file, "fasta"))

# iterate over every record in the fasta file
for fasta_record in fasta_records:

    # print fasta record id
    print fasta_record.id
    
    # query the results from NCBI
    result_handle = NCBIWWW.qblast("blastn", "nt", fasta_record)

    # save the results to a temporary file
    save_file = open("my_blast.xml", "w")
    save_file.write(result_handle.read())
    save_file.close()
    result_handle.close()
    
    # open the file again for interpretation
    result_handle = open("my_blast.xml")
    blast_records = NCBIXML.read(result_handle)
    
    # print the first match retrieved
    print 'Matches found:',len(blast_records.alignments)
    str = blast_records.alignments[0].title
    
    # convert from unicode to string
    str = str.encode()
    
    # cut out the descriptor field
    str = str.split('|')[4]
    str = str.split(' ')
    str = "Species match: %s %s" % (str[1],str[2])
    
    # return results
    print str</pre>

&nbsp;

&nbsp;