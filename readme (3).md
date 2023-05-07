# FOR ACE 
step 1 :   DOWNLOAD ace and its binary 
for downloading ace compressed folder : http://sweaglesw.org/linguistics/ace/download/ace-0.9.34-x86-64.tar.gz

download it and extract to any directory u want 

for ace english grammar (erg) :  http://sweaglesw.org/linguistics/ace/download/erg-2018-x86-64-0.9.34.dat.bz2

step2 : specify the ace path    :   for example - export PATH="/home/soumya/majorprojectjapen/ace-0.9.34:$PATH"

step 3 : run the command below just replace file paths as intended

we have to specify the english grammar file path here it is "/home/soumya/majorprojectjapen/ace-0.9.34/erg-files/erg-2018-x86-64-0.9.34.dat "

soumya@soumya-VirtualBox:~$ ace -g /home/soumya/majorprojectjapen/ace-0.9.34/erg-files/erg-2018-x86-64-0.9.34.dat -1Tf 
tom is a crazy man
SENT: tom is a crazy man
[ LTOP: h0
INDEX: e2 [ e SF: prop TENSE: pres MOOD: indicative PROG: - PERF: - ]
RELS: < [ proper_q<0:3> LBL: h4 ARG0: x3 [ x PERS: 3 NUM: sg IND: + ] RSTR: h5 BODY: h6 ]
 [ named<0:3> LBL: h7 CARG: "Tom" ARG0: x3 ]
 [ _be_v_id<4:6> LBL: h1 ARG0: e2 ARG1: x3 ARG2: x9 [ x PERS: 3 NUM: sg IND: + ] ]
 [ _a_q<7:8> LBL: h10 ARG0: x9 RSTR: h11 BODY: h12 ]
 [ _crazy_a_1<9:14> LBL: h13 ARG0: e14 [ e SF: prop TENSE: untensed MOOD: indicative PROG: bool PERF: - ] ARG1: x9 ]
 [ _man_n_1<15:18> LBL: h13 ARG0: x9 ] >
HCONS: < h0 qeq h1 h5 qeq h7 h11 qeq h13 >
ICONS: < > ]
NOTE: 1 readings, added 1351 / 249 edges to chart (109 fully instantiated, 82 actives used, 65 passives used)	RAM: 4396k


#  FOR JACY 

prerequisites : ace

STEP 1 : DOWNLOAD jacy from https://github.com/delph-in/jacy and extract it 

step 2 : we will have to create a jacy grammar file for that we will run following command :

ace -g /home/soumya/grammars/jacy/ace/config.tdl -G jacy.dat

here path refers to your directory path in which you have extracted 

step 3 :  we will have an jacy.dat now we will run following command in terminal : echo "いっしょ" | ace -g /home/soumya/grammars/jacy/ace/jacy.dat

for example :
soumya@soumya-VirtualBox:~$ echo "いっしょ" | ace -g /home/soumya/grammars/jacy/ace/jacy.dat
SENT: いっしょ
[ LTOP: h0 INDEX: e2 [ e TENSE: tense MOOD: indicative PROG: - PERF: - ASPECT: default_aspect PASS: - SF: prop ] RELS: < [ unknown_v_rel<0:4> LBL: h1 ARG0: e2 ARG: x4 [ x PERS: 3 ] ]  [ udef_q_rel<0:4> LBL: h5 ARG0: x4 RSTR: h6 BODY: h7 ]  [ "_いっしょ_n_unk_rel"<0:4> LBL: h8 ARG0: x4 ] > HCONS: < h0 qeq h1 h6 qeq h8 > ] ;  (96 frg-np -0.727376 0 1 (95 quantify-n-rule -0.322160 0 1 (4 いっしょ_n_1-tc 0.000000 0 1 ("いっしょ" 3 "token [ +FORM \"いっしょ\" +FROM \"0\" +TO \"4\" +ID diff-list [ LIST cons [ FIRST \"0\" REST list ] LAST list ] +POS pos [ +TAGS null +PRBS null ] +CLASS non_ne [ +INITIAL luk ] +TRAIT token_trait +PRED predsort +CARG \"いっしょ\" ]"))))
NOTE: 1 readings, added 21 / 2 edges to chart (3 fully instantiated, 2 actives used, 2 passives used)	RAM: 78k


NOTE: parsed 1 / 1 sentences, avg 78k, time 0.02880s

# important note for jacy :

sometimes here can be error such as NOTE: lexemes do not span position 0 `いっしょにいきましょう'!
NOTE: post reduction gap
SKIP: いっしょにいきましょう
NOTE: ignoring `いっしょにいきましょう'

it is caused by 2 errors : 
a) the input sentence is not correctly segmented : 
solution use mecab or other tokenisation tools

You can tokenize a sentence with mecab, and then parse it:

for installing mecab : pip install mecab 

echo "カタカナも漢字も大丈夫です。" | mecab -O wakati | ace -g /home/soumya/jacy.dat

mecab splits it into カタカナ も 漢字 も 大丈夫 です 。 which Jacy can then parse.

Of course, you can also split it by hand (and sometimes mecab splits it badly).   You can use a different tokenizer, but mecab is what we used.


b) another solution can be addition of the words into the lexicon for better scope 

you can add into lexicon by adding hpsc grammar rules in lexicon.tdl file 

for ex- 
sannen := ordinary-nohon-n-lex &
  [ STEM < "さんねん" >,
    SYNSEM.LKEYS.KEYREL.PRED "_sannen_a_1_rel",
   TRAITS native_token_list ].
 
sei := ordinary-nohon-n-lex &
  [ STEM < "せい" >,
    SYNSEM.LKEYS.KEYREL.PRED "_sei_n_1_rel",
    TRAITS native_token_list ].
 
desu := ordinary-nohon-n-lex &
  [ STEM < "です" >,
    SYNSEM.LKEYS.KEYREL.PRED "_desu_v_1_rel",
    TRAITS native_token_list ].


special note :  try to segment the sentence into words for the correct parsing .




#  FOR JAEN :


prerequisites : ace ,erg, jacy , pydelphin

step1 : download JaEn folder from https://github.com/delph-in/JaEn.git

step2 : compile the config-core.tdl from JaEn/ace/

for example :

ace -g jaen/ace/config-core.tdl -G jaen-core.dat

so after this we will get the jaen-core.dat which is the transfer grammar from japanese to english 


we have implemented JaEn in python using pydelphin  : 

step3: pip install pydelphin   # for installation of pydelphin

step4 : code for JaEn 

from delphin import ace
import cutlet
grm = '/home/soumya/majorprojectjapen/jacy/ace/jacy.dat'
conf = '/home/soumya/majorprojectjapen/jacy/ace/config.tdl'

jgrm = '/home/soumya/grammars/jaen-core.dat'
egrm = '/home/soumya/grammars/erg.dat'

n = str(input())

''' replace paths with ur directory paths'''

j_response = ace.parse('{}'.format(grm), n )
je_response = ace.transfer('{}'.format(jgrm), j_response.result(0)['mrs'])
e_response = ace.generate('{}'.format(egrm), je_response.result(0)['mrs'])


print(e_response.result(0)['surface'])


Note :

JaEn works for limited words and also keep in mind the issues which was encountered in jacy 








