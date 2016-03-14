#!/usr/bin/make -f
#
#   GNU Make Automation for Inform 7.
#
#   Copyright 2016 Alfie Wright.
#
#   This program is free software: you can redistribute it and/or modify
#   it under the terms of the GNU General Public License as published by
#   the Free Software Foundation, either version 3 of the License, or
#   (at your option) any later version.
#
#   This program is distributed in the hope that it will be useful,
#   but WITHOUT ANY WARRANTY; without even the implied warranty of
#   MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#   GNU General Public License for more details.
#
#   You should have received a copy of the GNU General Public License
#   along with this program.  If not, see <http://www.gnu.org/licenses/>.
#

#############################################################################
##  Replace the following with values suitable for your project.           ##
#############################################################################

        PROJECT=untitled_game
          TITLE=Untitled Game
         AUTHOR=Anonymous Author

     BUILD_TYPE=debug
   PROJECT_TYPE=gblorb
 INFORM7RELEASE=6L02

     INFORM7DIR=$(HOME)/Inform/$(INFORM7RELEASE)
           TIDY=/usr/bin/tidy
       XSLTPROC=/usr/bin/xsltproc
        UUIDGEN=/usr/bin/uuidgen

# Tips for users who don't want to learn Make:
#   - Once you've configured this file you can build your project
#       at any time by typing 'make' (or 'gmake' if GNU Make is not
#       the default make on your system) in the same directory as
#       this file.
#   - '#' characters are comments.
#   - Don't put spaces in the PROJECT field.
#   - The TITLE and AUTHOR fields are free text; enter anything.
#   - Enter all values without spaces before or after the value.
#       i.e. "A= B" and "A=B " are wrong,  "A=B" is right.
#   - The following values are currently understood for the following
#       fields (case-sensitive):
#
# BUILD_TYPE: debug | release
# PROJECT_TYPE: gblorb | zblorb
# INFORM7RELEASE: 6M62 | 6L38 | 6L02 | 6G60
#
#   - For the TIDY, XSLTPROC and UUIDGEN fields you can use the
#       'whereis' command to determine the correct path.
#       i.e. 'whereis tidy', 'whereis xsltproc', 'whereis uuidgen'.
#   - Don't worry if you are missing tidy or xsltproc, they
#       are only required for fixing the documentation Inform 7
#       generates.  The rest of the build will work without them.
#   - If you are missing uuidgen you will need to manually create a
#       text file called uuid.txt in your PROJECT.inform directory and
#       entering a uuid for your project (no spaces or new lines
#       allowed).  A tool like https://www.uuidgenerator.net/ may be
#       useful for this.
#   - You can call this file something other than GNUmakefile, if
#       you do then you can run it by typing 'make -f newfilename'
#       (or 'gmake -f newfilename') instead of 'make'.
#   - Renaming allows you to have multiple projects under one
#       directory if that's what you want.
#   - As an alternative to typing 'make -f somefile' you can make this
#       file executable and run it directly.  (if gmake is not the
#       default make on your system you will need to adjust the first
#       line of this file to point to it).
#   - This makefile generates a dependency file for all the extensions
#       used, but is unable to use it due to spaces in the filenames.
#   - This is _not_ how Makefiles should be written.  This is an 
#       extremely hacky and fragile set of scripts jammed into one
#       file.

#############################################################################
##  Leave the remaining lines alone unless you know what you're doing.     ##
#############################################################################

     PROJECTDIR=$(CURDIR)/$(PROJECT).inform
    MANIFESTDIR=$(CURDIR)/$(PROJECT).manifest
   MATERIALSDIR=$(CURDIR)/$(PROJECT).materials
           DEST=$(PROJECTDIR)/Build/$(PROJECT).$(PROJECT_TYPE)
           DOCS=$(shell find $(PROJECTDIR)/Index -type f -name "*.html")
   STORYLISTING=$(PROJECTDIR)/Index/Story.xhtml
     FIXDOCXSLT=$(CURDIR)/fixdocumentationpage.xslt
EXE_TYPE_gblorb=ulx
EXE_TYPE_zblorb=z5
       EXE_TYPE=$(EXE_TYPE_$(PROJECT_TYPE))
            EXE=$(PROJECTDIR)/Build/output.$(EXE_TYPE)

#############################################################################

                  NI=$(INFORM7DIR)/libexec/ni

        INFORM6_6M62=$(INFORM7DIR)/libexec/inform6
        INFORM6_6L38=$(INFORM7DIR)/libexec/inform6
        INFORM6_6L02=$(INFORM7DIR)/libexec/inform6
        INFORM6_6G60=$(INFORM7DIR)/libexec/inform-6.32-biplatform
             INFORM6=$(INFORM6_$(INFORM7RELEASE))

              CBLORB=$(INFORM7DIR)/libexec/cBlorb

        NIFLAGS_6M62=--internal $(INFORM7DIR)/share/inform7/Internal \
		     --project $(PROJECTDIR)
        NIFLAGS_6L38=--rules $(INFORM7DIR)/share/inform7/Inform7/Extensions \
		     --extension=$(EXE_TYPE) \
		     --package $(PROJECTDIR)
        NIFLAGS_6L02=--rules $(INFORM7DIR)/share/inform7/Inform7/Extensions \
		     --extension=$(EXE_TYPE) \
		     --package $(PROJECTDIR)
        NIFLAGS_6G60=--rules $(INFORM7DIR)/share/inform7/Inform7/Extensions \
		     --extension=$(EXE_TYPE) \
		     --package $(PROJECTDIR)
             NIFLAGS=$(NIFLAGS_$(INFORM7RELEASE))

INFORM6FLAGS_release=
  INFORM6FLAGS_debug=-SD

 INFORM6FLAGS_gblorb=-G
 INFORM6FLAGS_zblorb=-v8
        INFORM6FLAGS=$(INFORM6FLAGS_$(BUILD_TYPE)) $(INFORM6FLAGS_$(PROJECT_TYPE))

         CBLORBFLAGS=-unix

#############################################################################

export INFORM7RELEASE
export INFORM7DIR

.DELETE_ON_ERROR:
.PHONY: directories game docs all

all: directories game docs

game: $(DEST)

docs: $(STORYLISTING) $(DOCS:.html=.xhtml)

directories: $(PROJECTDIR) $(MANIFESTDIR) $(MATERIALSDIR) $(PROJECTDIR)/Source $(PROJECTDIR)/Build $(PROJECTDIR)/Release $(PROJECTDIR)/Index $(PROJECTDIR)/Index/Details

#############################################################################

$(PROJECTDIR):
	[ -d $@ ] || mkdir $@
	-touch $@

$(MANIFESTDIR):
	[ -d $@ ] || mkdir $@
	-touch $@

$(MATERIALSDIR):
	[ -d $@ ] || mkdir $@
	-touch $@

$(PROJECTDIR)/Source: $(PROJECTDIR)
	[ -d $@ ] || mkdir $@
	-touch $@

$(PROJECTDIR)/Build: $(PROJECTDIR)
	[ -d $@ ] || mkdir $@
	-touch $@

$(PROJECTDIR)/Release: $(PROJECTDIR)
	[ -d $@ ] || mkdir $@
	-touch $@

$(PROJECTDIR)/Index: $(PROJECTDIR)
	[ -d $@ ] || mkdir $@
	-touch $@

$(PROJECTDIR)/Index/Details: $(PROJECTDIR)/Index
	[ -d $@ ] || mkdir $@
	-touch $@

$(PROJECTDIR)/Source/story.ni: $(PROJECTDIR)/Source
	[ -f $@ ] || (echo \"$(TITLE)\" by $(AUTHOR); echo; echo startroom is a room.) > $@

$(PROJECTDIR)/uuid.txt:
	[ -f $@ -o ! -n "$(UUIDGEN)" -o ! -x "$(UUIDGEN)"  ] || echo -n `$(UUIDGEN)` > $@
	-touch $@

#############################################################################

$(PROJECTDIR)/Build/auto.inf: $(PROJECTDIR)/Source/story.ni $(PROJECTDIR)/uuid.txt
	$(NI) $(NIFLAGS)

$(PROJECTDIR)/Release.blurb: $(PROJECTDIR)/Build/auto.inf
	$(INFORM6) $(INFORM6FLAGS) $^ -o $(EXE)

$(DEST): $(PROJECTDIR)/Release.blurb
	$(CBLORB) $(CBLORBFLAGS) $< $@

#############################################################################

$(STORYLISTING): $(PROJECTDIR)/Source/story.ni
	awk "BEGIN { IFS=\"\"; OFS=\"\"; ORS=\"\"; print \"<!DOCTYPE html PUBLIC \"; print \"\\\"-//W3C//DTD XHTML 1.0 Strict//EN\\\" \"; print \"\\\"http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd\\\"\"; print \">\"; print \"<html xmlns=\\\"http://www.w3.org/1999/xhtml\\\">\"; print \"<head><title>Story Text</title>\"; print \"<style type=\\\"text/css\\\">\"; print \"head{margin:0;padding:0}\"; print \"body{margin:0;padding:8px}\"; print \"div:target{background:#aaaaff!important}div:nth-child(even){background:#f4f4f4}\"; print \"div:nth-child(odd){background:#e4e4e4}\"; print \"a{text-decoration:none}\"; print \"a:hover{text-decoration:underline}\"; print \"pre{font-size:10pt;width:100%;margin:0;padding:0}\"; print \"</style></head><body><pre>\"; } { print \"<div id=\\\"line\", NR,\"\\\">\"; print \"[<a href=\\\"#line\", NR, \"\\\">\"; printf \"%0.4d\", NR; print \"</a>]  \"; print; print \"</div>\"; } END { print \"</pre></body></html>\"; }" $< > $@

%.xhtml-tidy: %.html
	[ ! -n "$(TIDY)" -o ! -x "$(TIDY)" ] || $(TIDY) -clean -q -n -asxml -f /dev/null $^ > $@ || test $$? -eq 1

%.xhtml: %.xhtml-tidy $(FIXDOCXSLT)
	[ ! -n "$(XSLTPROC)" -o ! -x "$(XSLTPROC)" ] || $(XSLTPROC) -o $@ --novalid --param XHTMLStoryListing "'file://$(STORYLISTING)'" --param UserSpecificDocPath "'file://$(HOME)/Inform/Documentation'" --param VersionSpecificDocPath "'file://$(INFORM7DIR)/share/inform7/Documentation/'" $(FIXDOCXSLT) $<

#############################################################################

$(CURDIR)/$(PROJECT).deps: $(PROJECTDIR)/Source/story.ni
	awk -e "BEGIN { split(\"\", visited, \"\"); IGNORECASE = 1; RS = \"[.;]\"; PREVIOUS_FILENAME = \"\"; } \$$0 ~ /ends here\$$/ { nextfile; } { if (PREVIOUS_FILENAME != FILENAME) { visited [length (visited)] = FILENAME; PREVIOUS_FILENAME = FILENAME; ORS=\"\"; print \"\\n\\n\"; ORS=\" \"; print gensub (/ /, \"\\\\\\\\ \", \"g\", FILENAME) \":\"; } while (sub (/^[\\r\\n\\t ]*(volume|book|part|chapter|section|subsection|\"|\";)[^\\r\\n]*[\\r\\n\\t ]+/, \"\")); if (\$$1 !~ /include/) next; if (\$$2 ~ /\\(-/) next; if (\$$2 ~ /version/) i = 5; else i = 2; ext = \"\"; author = \"\"; found_by = 0; for (i; i <= NF; i++) { if (!\$$i) continue; if (\$$i ~ /by/) { found_by = 1; } else if (found_by) { if (author) author = author \" \"; author = author \$$i; } else { if (ext) ext = ext \" \"; ext = ext \$$i; } } path = author \"/\" ext \".i7x\"; if (system (\"[ -r \\\"$(MANIFESTDIR)/\" path \"\\\" ]\") == 0) { path = \"$(MANIFESTDIR)/\" path; } else if (system (\"[ -r \\\"$$HOME/Inform/Extensions/\" path \"\\\" ]\") == 0) { path = \"$$HOME/Inform/Extensions/\" path; } else if (system (\"[ -r \\\"$(INFORM7DIR)/share/inform7/Inform7/Extensions/\" path \"\\\" ]\") == 0) { path = \"$(INFORM7DIR)/share/inform7/Inform7/Extensions/\" path; } else { print \"Unable to find: \" path \"\\n\" > \"/dev/stderr\"; exit 1; } print gensub (/ /, \"\\\\\\\\ \", \"g\", path); if (!(path in visited || path in ARGV)) ARGV[++ARGC] = path; }" $< > $@

include $(CURDIR)/$(PROJECT).deps

#############################################################################

EXTRACTOR=awk -e "BEGIN { outside = 1; FS=\"\"; } \$$0 ~ /^\#Begin: $${TOKEN}/ { outside = 0;} \$$0 ~ /^\#End: $${TOKEN}/ { exit 0; } \$$0 ~ /^\# / { if (outside) next; sub (/^\# /, \"\"); print; }"

$(FIXDOCXSLT): $(MAKEFILE_LIST)
	TOKEN=DocFixer $(EXTRACTOR) $^ > $@

#Begin: DocFixer
# <xsl:stylesheet version="1.0"
#                 xmlns="http://www.w3.org/1999/xhtml"
#                 xmlns:h="http://www.w3.org/1999/xhtml"
#                 xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
# 
#   <xsl:output doctype-public="-//W3C//DTD XHTML 1.0 Transitional//EN"
#               doctype-system="http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"
#               encoding="utf-8"
#               indent="yes"
#               media-type="application/xhtml+xml"
#               method="xml"
#               omit-xml-declaration="no"
#               standalone="no"
#               version="1.0" />
# 
#   <!-- Arguments and Variables -->
#   <xsl:variable name="apos">'</xsl:variable>
#   <xsl:param name="XHTMLStoryListing"
#              required="yes" />
#   <xsl:param name="UserSpecificDocPath"
#              required="yes" />
#   <xsl:param name="VersionSpecificDocPath"
#              required="yes" />
# 
# 
# 
#   <!-- 'Folding' Boxes -->
#   <xsl:template match="h:p[child::h:a[starts-with(@onclick, 'showExtra')]]">
# 
#     <xsl:variable name="showExtraApos"
#                   select="concat ('showExtra(', $apos)" />
#     <xsl:variable name="showExtraID"
#                   select="substring-before (substring-after (child::h:a[@onclick]/@onclick, $showExtraApos), $apos)" />
# 
#     <input class="hideShow" type="checkbox">
#       <xsl:attribute name="id">
#         <xsl:value-of select="$showExtraID" />
#       </xsl:attribute>
#     </input>
# 
#     <p>
#       <xsl:attribute name="class">hideShowLabelP 
#       <xsl:value-of select="@class" /></xsl:attribute>
#       <label class="hideShowLabel">
#         <xsl:attribute name="for">
#           <xsl:value-of select="$showExtraID" />
#         </xsl:attribute>
#         <xsl:apply-templates select="child::*[not(self::h:a)]|text()" />
#       </label>
#     </p>
# 
#     <div class="extra">
#       <xsl:apply-templates select="../*[@id=$showExtraID]/h:table/h:tr[2]/h:td[2]/*" />
#     </div>
# 
#   </xsl:template>
# 
# 
# 
#   <!-- Structural Templates -->
#   <xsl:template match="h:head">
#     <head>
#       <xsl:apply-templates select="node()" />
#       <style type="text/css">
#         .extra{
#         margin:8px;
#         padding:8px 8px 8px 50px;
#         background-color:#ccc;
#         border-radius:8px;
#         }
#         .hideShow{
#         position:absolute;
#         left:-8px;
#         width:1px;
#         overflow:hidden;
#         }
#         .hideShowLabel{
#         background-repeat:no-repeat;
#         padding:0 0 0 10px;
#         background-position:left center;
# 	cursor: pointer;
#         }
#         .hideShow:not(:checked) + .hideShowLabelP + .extra{
#         display:none;
#         }
#         .hideShow:not(:checked)+.hideShowLabelP&gt;.hideShowLabel{
#         background-image:url('<xsl:value-of select="concat ($VersionSpecificDocPath,'/doc_images/extra.png')" />');
#         }
#         .hideShow:checked+.hideShowLabelP&gt;.hideShowLabel{
#         background-image:url('<xsl:value-of select="concat ($VersionSpecificDocPath,'/doc_images/extraclose.png')" />');
#         }
#         .newbox a:link{
#         text-decoration:none;
#         }
#         .newbox a:visited{
#         text-decoration:none;
#         }
#         .newbox a:active{
#         text-decoration:none;
#         }
#         .newbox a:hover{
#         text-decoration:none;
#         color:#444444;
#         }
#         .blocklink{
#         display:block;
#         position:absolute;
#         top:0;
#         left:0;
#         bottom:0;
#         right:0;
#         width:auto;
#         height:auto;
#         }
#         .newheadingboxhighimage{
#         display:table-cell;
#         margin:0;
#         width:117px;
#         background:url('<xsl:value-of select="concat ($VersionSpecificDocPath,'/doc_images/index@2x.png')" />') center center no-repeat;
#         }
#         .newheadingboxhigh{
#         display:table-cell;
#         margin:0;
#         position:relative;
#         height:117px;
#         padding:0px;
#         white-space:nowrap;
#         background:#eeeeee;
#         font-family:"Lucida Grande","Lucida Sans Unicode",Helvetica,Arial,Verdana,sans-serif;
#         -webkit-font-smoothing:antialiased;
#         }
#         .newsymbol{
#         position:absolute;
#         top:-4px;
#         left:-1px;
#         width:100%;
#         color:#ffffff;
#         padding:14px 0px 14px 1px;
#         font-size:20px;
#         font-weight:bold;
#         text-align:center;
#         }
#         .newindexno{
#         position:absolute;
#         top:1px;
#         left:3px;
#         color:#ffffff;
#         font-size:7pt;
#         text-align:left;
#         }
#         .newrubric{
#         position:absolute;
#         top:35px;
#         width:100%;
#         color:#ffffff;
#         font-size:9px;
#         font-weight:bold;
#         text-align:center;
#         }
#         .newbox{
#         position:relative;
#         display:table-cell;
#         margin:0;
#         height:56px;
#         width:56px;
#         padding:0;
#         font-family:"Lucida Grande","Lucida Sans Unicode",Helvetica,Arial,Verdana,sans-serif;
#         background:blue;
#         -webkit-font-smoothing:antialiased;
#         }
#         .newheadingbox{
#         display:table-cell;
#         position:relative;
#         margin:0;
#         font-family:"Lucida Grande","Lucida Sans Unicode",Helvetica,Arial,Verdana,sans-serif;
#         -webkit-font-smoothing:antialiased;
#         white-space:nowrap;
#         background:#eeeeee;
#         color:#222222;
#         padding:14px 10px 0px 10px;
#         font-size:20px;
#         font-weight:bold;
#         }
#         .newheadingtext{
#         position:absolute;
#         top:-4px;
#         left:-1px;
#         width:100%;
#         color:#222222;
#         padding:14px 10px 0px 10px;
#         font-size:20px;
#         font-weight:bold;
#         }
#         .newheadingrubric{
#         position:absolute;
#         top:36px;
#         width:100%;
#         color:#222222;
#         font-size:11px;
#         font-weight:bold;
#         padding:0px 10px 0px 10px;
#         }
#         .newperiodictablerow{
#         display:table;
#         margin:0 0 -4px 0;
#         padding:0;
#         width:100%;
#         border-spacing:4px;
#         }
#         .newperiodictable{
#         display:block;
#         margin:0;
#         padding:0;
#         }
#         .newsidebar{
#         position:relative;
#         width:16px;
#         background:#888;
#         display:table-cell;
#         margin:0;
#         padding:0;
#         }
#         .newsidebarhigh{
#         width:16px;
#         display:table-cell;
#         margin:0;
#         padding:0;
#         }
#       </style>
#     </head>
#   </xsl:template>
# 
#   <xsl:template match="h:body">
#     <body>
#       <form>
#         <xsl:apply-templates select="node()|text()" />
#       </form>
#     </body>
#   </xsl:template>
# 
# 
# 
#   <!-- Copying Periodic Tables -->
#   <xsl:template match="*[@class='sidebar']">
#     <li class="newsidebar">
#       <a class="blocklink">
#         <xsl:attribute name="href">
#           <xsl:call-template name="extract_URL_from_onclick"
#                              select="." />
#         </xsl:attribute>
#       </a>
#     </li>
#   </xsl:template>
#   <xsl:template match="*[@class='headingbox']">
#     <li class="newheadingbox">
#       <a class="blocklink">
#         <xsl:attribute name="href">
#           <xsl:call-template name="extract_URL_from_onclick"
#                              select="." />
#         </xsl:attribute>
#         <span class="newheadingtext">
#           <xsl:copy-of select="*[@class='headingtext']/text()" />
#         </span>
#         <span class="newheadingrubric">
#           <xsl:copy-of select="*[@class='headingrubric']/text()" />
#         </span>
#       </a>
#     </li>
#   </xsl:template>
#   <xsl:template match="*[@class='headingboxhigh']">
#     <li class="newsidebarhigh" />
#     <li class="newheadingboxhighimage" />
#     <li class="newheadingboxhigh">
#       <span class="newheadingtext">
#         <xsl:copy-of select="*[@class='headingtext']/text()" />
#       </span>
#       <span class="newheadingrubric">
#         <xsl:copy-of select="*[@class='headingrubric']/text()" />
#       </span>
#     </li>
#   </xsl:template>
#   <xsl:template match="*[@class='box']">
#     <li class="newbox">
#       <xsl:attribute name="id">
#         <xsl:value-of select="@id" />
#       </xsl:attribute>
#       <a class="blocklink">
#         <xsl:attribute name="href">
#           <xsl:call-template name="extract_URL_from_onclick"
#                              select="." />
#         </xsl:attribute>
#         <span class="newsymbol">
#           <xsl:value-of select="*[@class='symbol']/text()" />
#         </span>
#         <span class="newindexno">
#           <xsl:value-of select="*[@class='indexno']/text()" />
#         </span>
#         <span class="newrubric">
#           <xsl:value-of select="*[@class='rubric']/text()" />
#         </span>
#       </a>
#     </li>
#   </xsl:template>
#   <xsl:template match="h:tr[../../@id='periodictable']">
#     <ul class="newperiodictablerow">
#       <xsl:apply-templates select="h:td/*[@class='box' or @class='headingbox' or @class='sidebar' or @class='headingboxhigh']" />
#     </ul>
#   </xsl:template>
#   <xsl:template match="*[@id='periodictable']/h:table">
#     <div class="newperiodictable">
#       <xsl:apply-templates select="h:tr" />
#     </div>
#   </xsl:template>
# 
# 
# 
#   <!-- URL utilities -->
#   <xsl:template name="extract_URL_from_onclick">
#     <xsl:choose>
#       <xsl:when test="starts-with(parent::*/@onclick, 'window.location')">
#         <xsl:call-template name="extract_URL_from_onclick_window_location" />
#       </xsl:when>
#       <xsl:when test="starts-with(parent::*/@onclick, 'click_element_box')">
#         <xsl:call-template name="extract_URL_from_click_element_box" />
#       </xsl:when>
#     </xsl:choose>
#   </xsl:template>
# 
#   <xsl:template name="extract_URL_from_onclick_window_location">
#     <xsl:variable name="url"
#                   select="substring-before (substring-after (parent::*/@onclick, $apos), $apos)" />
# 
#     <xsl:choose>
#       <xsl:when test="contains ($url, '?')">
#         <xsl:variable name="urlPage"
#                       select="substring-before ($url, '?')" />
#         <xsl:variable name="urlQuery"
#                       select="substring-after ($url, '?')" />
#         <xsl:variable name="urlNewPage"
#                       select="concat (substring-before ($urlPage, '.html'), '.xhtml')" />
#         <xsl:value-of select="concat ($urlNewPage, '#', $urlQuery)" />
#       </xsl:when>
#       <xsl:otherwise>
#         <xsl:value-of select="concat (substring-before ($url, '.html'), '.xhtml')" />
#       </xsl:otherwise>
#     </xsl:choose>
#   </xsl:template>
# 
#   <xsl:template name="extract_URL_from_click_element_box">
#     <xsl:variable name="url"
#                   select="substring-before (substring-after (parent::*/@onclick, $apos), $apos)" />
# 
#     <xsl:value-of select="concat ('#', $url)" />
#   </xsl:template>
# 
#   <xsl:template name="replace_html_suffix_with_xhtml">
#     <xsl:param name="url" required="yes" />
#     <xsl:choose>
#       <xsl:when test="contains($url,'.html')">
#         <xsl:value-of select="concat (substring-before ($url,'.html'),'.xhtml')"/>
#       </xsl:when>
#       <xsl:otherwise>
#         <xsl:value-of select="$url" />
#       </xsl:otherwise>
#     </xsl:choose>
#   </xsl:template>
# 
#   <xsl:template name="expand_inform_URL">
#     <xsl:choose>
#       <xsl:when test="starts-with(.,'inform:')">
# 	<xsl:choose>
#           <xsl:when test="starts-with(.,'inform://Extensions')">
# 	    <xsl:value-of select="$UserSpecificDocPath" />
#             <xsl:call-template name="replace_html_suffix_with_xhtml">
#               <xsl:with-param name="url"
#                               select="substring-after(., 'inform://Extensions')" />
#             </xsl:call-template>
#           </xsl:when>
#           <xsl:otherwise>
# 	    <xsl:value-of select="$VersionSpecificDocPath" />
#             <xsl:call-template name="replace_html_suffix_with_xhtml">
#               <xsl:with-param name="url"
#                               select="substring-after(., 'inform:')" />
#             </xsl:call-template>
#           </xsl:otherwise>
# 	</xsl:choose>
#       </xsl:when>
#       <xsl:when test="starts-with(., 'source:story.ni')">
# 	<xsl:value-of select="$XHTMLStoryListing" />
#         <xsl:value-of select="concat('#', substring-after(., '#'))" />
#       </xsl:when>
#       <xsl:otherwise>
#         <xsl:value-of select="." />
#       </xsl:otherwise>
#     </xsl:choose>
#   </xsl:template>
# 
# 
# 
#   <!-- Create a style attribute -->
#   <xsl:template name="collate_styles">
#     <xsl:if test="@style|@background|@cellpadding|@cellspacing|@width|@height">
#       <xsl:attribute name="style">
#         <xsl:apply-templates mode="collate_style" select="@*"/>
#       </xsl:attribute>
#     </xsl:if>
#   </xsl:template>
# 
#   <xsl:template mode="collate_style" match="@background">
#     <xsl:value-of select="concat ('background-image: url(', $apos, $VersionSpecificDocPath, substring-after(., 'inform:'), $apos, ');')" />
#   </xsl:template>
#   <xsl:template mode="collate_style" match="@style">
#     <xsl:value-of select="concat (., ';')" />
#   </xsl:template>
#   <xsl:template mode="collate_style" match="@cellpadding">
#     <xsl:value-of select="concat ('padding:', ., 'px;')" />
#   </xsl:template>
#   <xsl:template mode="collate_style" match="@cellspacing">
#     <xsl:value-of select="concat ('border-collapse:separate;border-spacing:', ., 'px;')" />
#   </xsl:template>
#   <xsl:template mode="collate_style" match="@width">
#     <xsl:text>width:</xsl:text>
#     <xsl:value-of select="." />
#     <xsl:if test="not(contains (., '%'))"><xsl:text>px</xsl:text></xsl:if>
#     <xsl:text>;</xsl:text>
#   </xsl:template>
#   <xsl:template mode="collate_style" match="@height">
#     <xsl:text>height:</xsl:text>
#     <xsl:value-of select="." />
#     <xsl:if test="not(contains(., '%'))"><xsl:text>px</xsl:text></xsl:if>
#     <xsl:text>;</xsl:text>
#   </xsl:template>
#   <xsl:template mode="collate_style" match="@border">
#     <xsl:value-of select="concat ('border-width:', ., 'px;')" />
#   </xsl:template>
#   <xsl:template mode="collate_style" match="@*" />
# 
# 
# 
#   <!-- Things to drop -->
#   <xsl:template match="h:*[starts-with(@id, 'extra')]" />
#   <xsl:template match="h:*[starts-with(@href,'javascript:')]" />
#   <xsl:template match="h:script" />
#   <xsl:template match="h:meta" />
#   <xsl:template match="@border" />
#   <xsl:template match="@name" />
#   <xsl:template match="@onclick" />
#   <xsl:template match="@style" />
#   <xsl:template match="@background" />
#   <xsl:template match="@cellpadding" />
#   <xsl:template match="@cellspacing" />
#   <xsl:template match="@width" />
#   <xsl:template match="@height" />
# 
# 
# 
#   <!-- Fix URLs -->
#   <xsl:template match="@href">
#     <xsl:attribute name="href">
#       <xsl:call-template name="expand_inform_URL" /><!--_and_replace_html_suffix_with_xhtml"-->
#     </xsl:attribute>
#   </xsl:template>
#   <xsl:template match="@src">
#     <xsl:attribute name="src">
#       <xsl:call-template name="expand_inform_URL" />
#     </xsl:attribute>
#   </xsl:template>
# 
# 
# 
# 
#   <!-- The Identity Transforms -->
#   <xsl:template match="node ()|@*">
#     <xsl:copy>
#       <xsl:call-template name="collate_styles" />
#       <xsl:apply-templates select="node ()|@*" />
#     </xsl:copy>
#   </xsl:template>
# 
# </xsl:stylesheet>
#End: DocFixer
