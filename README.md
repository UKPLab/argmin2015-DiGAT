# Discourse Graph Annotation Tool (DiGAT)

Annotation tool used in the following paper:

> Kirschner, C., Eckle-Kohler, J., & Gurevych, I. (2015). Linking the Thoughts : Analysis of Argumentation Structures in Scientific Publications. In Proceedings of the 2nd Workshop on Argumentation Mining (pp. 1–11). Denver, Colorado: Association for Computational Linguistics. URL: http://www.aclweb.org/anthology/W15-0501

Copyright (c) 2013-2015 UKP TU Darmstadt: Artem Vovk, Roland Kluge, Christian Kirschner

Licenced under ASL 2.0

## Installation

* Download and Install (=unpack) Google App Engine for Python in a folder $GOOGLE_APPENGINE$: https://developers.google.com/appengine/downloads
* Download the source code for DiGAT to $GOOGLE_APPENGINE$/DiGAT
* Edit the file $GOOGLE_APPENGINE$/DiGAT/au/data/access_control.py
  * add your google account mail address (and the address of all people who will use the tool) to user_activation and set the permissions: 

        // Stores for each user whether the user is activated
        user_activation = {"eckle.kohler@gmail.com" : True, "kirschner.chr@gmail.com" : True}

        ALL_VIEWS = [HOME_VIEW_ID, ANNOTATE_VIEW_ID, COMPARE_VIEW_ID, MANAGE_DATA_VIEW_ID]
        // Stores for each user the list of views that he/she can access
        permissions = {"eckle.kohler@gmail.com" : [HOME_VIEW_ID, ANNOTATE_VIEW_ID], "kirschner.chr@gmail.com" : ALL_VIEWS}

* Run the tool locally:
  * cd $GOOGLE_APPENGINE$/
  * ./dev_appserver.py DiGAT/
    * You can access the tool here: http://localhost:8080/createdata
    * Log in with your google mail address which you added in the access_control file before 
  * If you want to make the tool available online, go to http://www.appspot.com, log in with your google account and create a new Application (otherwise you still can run it locally)
    * Upload the tool to your online available appengine (you have to repeat this step always if you change something in $GOOGLE_APPENGINE$/DiGAT (e.g. adding data or changing users/permissions):
      * cd $GOOGLE_APPENGINE$/
      * ./appcfg.py update DiGAT/ 

## Data import

* Input text have to be in the following JSON format (there is one example json document in $GOOGLE_APPENGINE$/DiGAT/data):

        {"text":"%see below%", "title":"Homogene Begabtenklassen am Gymnasium", "num_sentences":"163", "url":"HOCHBEGABUNG_1.html.xml"}

* Note: The part before "_" in url is the class the text belongs to, the part after "_" is the concrete identifier for the text (for example you could add different texts 1, 2, 3 with topic "HOCHBEGABUNG")

* The text has to be in the following format:

        <h2>Heading<\/h2>
        <span class=\"sentence\" idx=\"1\">Sentence<\/span> (idx must be the same as the idx of the first Token in the sentence)
        <span class=\"token\" idx=\"1\">Token<\/span> (with unique idx)
        <span class=\"gap\"> <\/span> (for space character)
        <p>Paragraph<\/p>
        \n (for a new line, usually between two paragraphs)
        <b><\/b> (for highlighting something) 

* Place the texts (in JSON format) which have to be annotated with the tool in $GOOGLE_APPENGINE$/DiGAT/data
* To import the texts into the tool start the tool and log in (see Installation) go to "Manage Data" (MANAGE_DATA_VIEW_ID or ALL_VIEWS permission must be set for this user) and click on "Load data". If you use it online don't forget to upload with ./appcfg.py update DiGAT/
* You can create the input files for example with a DKPRO Pipeline which performs Sentence Splitting, Tokenizing etc. and writes the data in the format described above 

## Data export

* Go to "Manage Data" and click on "Dump Corpus"
* Format: ... 

## Annotating texts

* To annotate a text go to "Annotate" and select a document
* To annotate one text snippet (we call it concept) with several argument components and relations (for example two paragraphs), first click on the first token and then on the last token of the snippet (it must start and end with a full sentence). Each sentence is automatically annotated as one argument component with one node in the graph on the right hand side.
* To add a relation between two argument components first click on the source component and then on the target component and select the relation type in the uppopping menue
* To remove argument components double-click on the component
* To view the graph and the relations for one concept right-click on one argument component of the conept
* Don't forget to "Save" your annotations before leaving
* With "Approve" you lock the document. Only the administrator can unapprove the document in the "Manage Data" view. 