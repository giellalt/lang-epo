# This is a makefile that builds the EPO morphological parser
# *****************************************************************
# This is a dummy file. EPO refers to the name of the language, epo to the 
# ISO code of the language, for use in file names.

# This is a preliminary file that builds a EPO parser based upon
# xfst and not twolc.

# Here we build the final generator , an inverted transducer of the analyzer.
# It is dependent upon epo.save

iepo.fst: ../bin/iepo.fst
../bin/iepo.fst: ../bin/epo.fst
	@echo
	@echo "*** Building the inverse iepo.fst ***"
	@echo
	@printf "load ../bin/epo.save \n\
	invert net \n\
	save stack ../bin/iepo.fst \n\
	quit \n" > ../tmp/iepo-fst-script
	@xfst < ../tmp/iepo-fst-script
	@rm -f ../tmp/iepo-fst-script


# This goal is to build the final analyser. It depends on all the files.

epo.fst: ../bin/epo.fst
../bin/epo.fst: ../bin/epo.save ../bin/caseconv.fst ../bin/tok.fst 
	@echo
	@echo "*** Building epo.fst ***" ;
	@echo
	@printf "read regex [[@\"../bin/epo.save\"] .o. \
	[@\"../bin/caseconv.fst\"]] ; \n\
	save stack ../bin/epo.fst \n\
	quit \n" > ../tmp/epo-fst-script
	@xfst < ../tmp/epo-fst-script
	@rm -f ../tmp/epo-fst-script


# The second goal is to build the caseconv.fst file
# This goal depends on case.regex

caseconv.fst: ../bin/caseconv.fst
../bin/caseconv.fst: case.regex
	@echo
	@echo "*** Building caseconv.fst ***" ;
	@echo
	@printf "read regex < case.regex \n\
	save stack ../bin/caseconv.fst \n\
	quit \n" > ../tmp/caseconv-script
	@xfst < ../tmp/caseconv-script
	@rm -f ../tmp/caseconv-script


# Another goal is to build a preprocessor.This goal depends on tok.txt

tok.fst: ../bin/tok.fst
../bin/tok.fst: tok.txt
	@echo
	@echo "*** Building the tokenizer tok.fst ***" ;
	@echo
	@printf "source tok.txt \n\
	save stack ../bin/tok.fst \n\
	quit \n" > ../tmp/tok-script
	@xfst < ../tmp/tok-script
	@rm -f ../tmp/tok-script


epo.save: ../bin/epo.save
../bin/epo.save: ../bin/xfst-epo.bin ../bin/epo-lex.save
	@echo
	@echo "*** Building the parser epo.save ***"
	@echo
	@printf "read regex [[@\"../bin/epo-lex.save\"] .o. \
	[@\"../bin/xfst-epo.bin\"]] ; \n\
	save stack ../bin/epo.save \n\
	quit \n" > ../tmp/epo-save-script
	@xfst < ../tmp/epo-save-script
	@rm -f ../tmp/epo-save-script

# The first goal is to build epo-lex.save
# This goal depends on a bunch of lexicon files

epo-lex.save: ../bin/epo-lex.save
../bin/epo-lex.save: epo-lex.txt noun-epo-morph.txt noun-epo-lex.txt \
        adj-epo-lex.txt \
	adv-epo-lex.txt verb-epo-lex.txt pron-epo-lex.txt punct-epo-lex.txt \
	pp-epo-lex.txt
	@echo
	@echo "*** Building epo-lex.save ***" ;
	@echo
	printf "compile-source epo-lex.txt noun-epo-morph.txt noun-epo-lex.txt \
	adj-epo-lex.txt verb-epo-lex.txt verb-epo-morph.txt pron-epo-lex.txt \
	adv-epo-lex.txt pp-epo-lex.txt punct-epo-lex.txt \n\
	save-source ../bin/epo-lex.save \n\
	quit \n" > ../tmp/epo-lex-save-script
	@lexc < ../tmp/epo-lex-save-script
	rm -f ../tmp/epo-lex-save-script

# The second goal is to build xfst-epo.bin
# This goal depends on xfst-epo.txt

xfst-epo.bin: ../bin/xfst-epo.bin
../bin/xfst-epo.bin: xfst-epo.txt
	@echo
	@echo "*** Building xfst-epo.bin ***" ;
	@echo
	@printf "source xfst-epo.txt \n\
	save stack ../bin/xfst-epo.bin \n\
	quit \n" > ../tmp/xfst-epo-script
	@xfst < ../tmp/xfst-epo-script
	@rm -f ../tmp/xfst-epo-script

clean:
	@rm -f ../bin/*.bin ../bin/*.fst ../bin/*.save


