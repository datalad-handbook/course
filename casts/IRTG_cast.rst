say 'Datasets are datalads core data type. We will explore the concepts of datasets by creating one with datalad create. optional configuration template and a description'
run 'datalad create --description "course on DataLad-101 on my private Laptop" -c text2git DataLad-101'
say 'Datalad informs about what it is doing during a command. At the end is a summary, in this case it is ok. What is inside of a newly created dataset? We list contents with ls.'
run 'cd DataLad-101
ls    # ls does not show any output, because the dataset is empty.'
say 'GIT LOG, SHASUM, MESSAGE: A dataset is version controlled. This means, edits done to a file are associated with information about the change, the author, and the time + ability to restore previous states of the dataset. Let'"'"'s take a look into the history, even if it is small atm'
run 'git log'
say 'Datalad, git-annex, and git create hidden files and directories in your dataset. Make sure to not delete them!'
run 'ls -a # show also hidden files'
say 'The dataset is empty, lets put some PDFs inside. First, create a directory to store them in:'
run 'mkdir books'
say 'The tree command shows us the directory structure in the dataset. Apart from the directory, its empty.'
run tree
say 'We use wget to download a few books from the web. CAVE: longish realcommand!'
run 'cd books && wget -nv https://sourceforge.net/projects/linuxcommand/files/TLCL/19.01/TLCL-19.01.pdf/download -O TLCL.pdf && wget -nv https://www.gitbook.com/download/pdf/book/swaroopch/byte-of-python -O byte-of-python.pdf && cd ../'
say 'Here they are:'
run tree
say 'What has happened to our dataset now with this new content? We can use datalad status to find out:'
run 'datalad status'
say 'ATM the files are untracked and thus unknown to any version control system. In order to version control the PDFs we need to save them. We attach a meaningful summary of this with the -m option:'
run 'datalad save -m "add books on Python and Unix to read later"'
say 'Save command reports what has been added to the dataset. Now we can see how this action looks like in our dataset'"'"'s history:'
run 'git log -p -n 1'
say 'Its inconvenient that we saved two books together - we should have saved them as independent modifications of the dataset. To see how single modifications can be saved, let'"'"'s download another book'
run 'cd books && wget -nv https://github.com/progit/progit2/releases/download/2.1.154/progit.pdf && cd ../'
say 'Check the dataset state with the status command frequently'
run 'datalad status'
say 'To save a single modification, provide a path to it!'
run 'datalad save -m "add reference book about git" books/progit.pdf'
say 'Let'"'"'s view the growing history (concise with the --oneline option):'
run '# lets make the output a bit more concise with the --oneline option
git log --oneline'
say 'Let'"'"'s find out how we can modify files in dataset. Lets create a text file with notes about the DataLad commands we learned. (maybe explain here docs)'
run 'cat << EOT > notes.txt
One can create a new dataset with '"'"'datalad create [--description] PATH'"'"'.
The dataset is created empty

EOT'
say 'As expected, there is a new file in the dataset. At first the file is untracked. We can save without a path specification because it is the only existing modification'
run 'datalad status'
run 'datalad save -m "Add notes on datalad create"'
say 'Now let'"'"'s start to modify this text file by adding more notes to it. Think about this being a code file that you add functions to:'
run 'cat << EOT >> notes.txt
The command "datalad save [-m] PATH" saves the file
(modifications) to history. Note to self:
Always use informative, concise commit messages.

EOT'
run 'datalad status'
say 'The modification can simply be saved as well'
run 'datalad save -m "add note on datalad save"'
say 'An the history gives an accurate record of what happened to this file'
run 'git log -p -n 2'
say 'The next challenge is to install an existing dataset from the web as a subdataset. First, we create a location for this'
run '# we are in the root of DataLad-101
mkdir recordings'
say 'We need to install the dataset as a subdataset. For this, we use the datalad install command with a --dataset option and --source option as well as a path. Else the dataset would not be registered as a subdataset!'
run 'datalad install -d . -s https://github.com/datalad-datasets/longnow-podcasts.git recordings/longnow'
say 'Let'"'"'s take a look at the directory structure after the installation'
run 'tree -d   # we limit the output to directories'
say 'And now lets look into these seminar series folders: There are hundreds of mp3 files, yet the download only took a few seconds! How can that be?'
run 'cd recordings/longnow/Long_Now__Seminars_About_Long_term_Thinking
ls'
say 'Upon installation of a DataLad dataset, DataLad retrieves only small files and metadata. Therefore the dataset is tiny in size. The files are non-functional now atm (Try opening one)'
run 'cd ../      # in longnow/
du -sh      # Unix command to show size of contents'
say 'But how large would the dataset be if we had all the content?'
run 'datalad status --annex'
say 'Now let'"'"'s finally get some content in this dataset. This is done with the datalad get command'
run 'datalad get Long_Now__Seminars_About_Long_term_Thinking/2003_11_15__Brian_Eno__The_Long_Now.mp3'
say 'Datalad status can also summarize how much of the content is already present locally:'
run 'datalad status --annex all'
say 'Let'"'"'s get a few more files. Note how already obtained files are not downloaded again:'
run 'datalad get Long_Now__Seminars_About_Long_term_Thinking/2003_11_15__Brian_Eno__The_Long_Now.mp3 \
Long_Now__Seminars_About_Long_term_Thinking/2003_12_13__Peter_Schwartz__The_Art_Of_The_Really_Long_View.mp3 \
Long_Now__Seminars_About_Long_term_Thinking/2004_01_10__George_Dyson__There_s_Plenty_of_Room_at_the_Top__Long_term_Thinking_About_Large_scale_Computing.mp3'
say 'On Dataset nesting: You have seen the history of DataLad-101. But the subdataset has a standalone history as well! We can find out who created it!'
run 'git log --reverse'
say 'We can make a note about this:'
run '# in the root of DataLad-101:
cd ../../
cat << EOT >> notes.txt
The command '"'"'datalad install [--source] PATH'"'"'
installs a dataset from e.g., a URL or a path.
If you install a dataset into an existing
dataset (as a subdataset), remember to specify the
root of the superdataset with the '"'"'-d'"'"' option.

EOT
datalad save -m "Add note on datalad install"'
say 'The superdataset only stores the version of the subdataset.  Let'"'"'s take a look into how the superdataset'"'"'s history looks like'
run 'git log -p'
say 'We can find this shasum in the subdatasets history: it'"'"'s the most recent change'
run 'cd recordings/longnow
git log --oneline'
run 'cd ../../'
say 'its impossible to remember how a large data analysis produced which result for a human, but datalad can help to keep track. To see this in action, we'"'"'ll do a data analysis. Start with yoda principles and structure ds with code directory.'
run '### Code snippet 37
mkdir code && tree -d'
say 'We will create a script to execute. Let'"'"'s make one that summarizes the podcasts titles in the longnow dataset:'
run '### Code snippet 38
cat << EOT > code/list_titles.sh
for i in recordings/longnow/Long_Now__Seminars*/*.mp3; do
   # get the filename
   base=\$(basename "\$i");
   # strip the extension
   base=\${base%.mp3};
   # date as yyyy-mm-dd
   printf "\${base%%__*}\t" | tr '"'"'_'"'"' '"'"'-'"'"';
   # name and title without underscores
   printf "\${base#*__}\n" | tr '"'"'_'"'"' '"'"' '"'"';
done
EOT'
say 'We have to save the script first: status and save'
run '### Code snippet 39
datalad status'
say '... preferably with a helpful commit message'
run '### Code snippet 40
datalad save -m "Add simple script to write a list of podcast speakers and titles"'
say 'The datalad run command records a command'"'"'s impact on a dataset. We try it in the most simple way:'
run '### Code snippet 41
datalad run -m "create a list of podcast titles" "bash code/list_titles.sh > recordings/podcasts.tsv"'
say 'Let'"'"'s now check what has been written into the history. (runrecord)'
run '### Code snippet 42
git log -p -n 1'
say 'A run command that does not result in changes (no modifcations, no additional files) will not produce a record in the dataset history. So what happens if we do the same again?'
run '### Code snippet 43
datalad run -m "Try again to create a list of podcast titles" "bash code/list_titles.sh > recordings/podcasts.tsv"'
say 'as the result is byte-identical, there is no new commit'
run '### Code snippet 44
git log --oneline'
say 'The script produced a simple list of podcast titles. let'"'"'s take a look into our output file. What'"'"'s cool is that is was created in a way that the code and output are linked:'
run '### Code snippet 45
less recordings/podcasts.tsv'
say 'Dang, we made a mistake in our script: we only listed a part of the podcasts! Let'"'"'s fix the script:'
run '### Code snippet 46
cat << EOT >| code/list_titles.sh
for i in recordings/longnow/Long_Now*/*.mp3; do
   # get the filename
   base=\$(basename "\$i");
   # strip the extension
   base=\${base%.mp3};
   printf "\${base%%__*}\t" | tr '"'"'_'"'"' '"'"'-'"'"';
   # name and title without underscores
   printf "\${base#*__}\n" | tr '"'"'_'"'"' '"'"' '"'"';

done
EOT'
run '### Code snippet 47
datalad status'
run '### Code snippet 48
datalad save -m "BF: list both directories content" code/list_titles.sh'
say 'We could execute the same command as before. However, we can also let DataLad take care of it, and use the datalad rerun command.'
run '### Code snippet 49
git log -n 2'
say 'We'"'"'ll find the shasum of the run commit and plug it into rerun'
run '### Code snippet 50
echo "$ datalad rerun $(git rev-parse HEAD~1)" && datalad rerun $(git rev-parse HEAD~1)'
say 'how does a rerun look in the history?'
run '### Code snippet 51
git log -n 1'
say 'The datalad diff command can help us find out what changed between the last two commands:'
run '### Code snippet 52
datalad diff --to HEAD~1'
say 'The git diff command has even more insights:'
run '### Code snippet 53
git diff HEAD~1'