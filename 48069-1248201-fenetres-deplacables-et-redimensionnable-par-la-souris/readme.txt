Fenetres déplacables et redimensionnable par la souris------------------------------------------------------
Url     : http://codes-sources.commentcamarche.net/source/48069-fenetres-deplacables-et-redimensionnable-par-la-sourisAuteur  : pdc_666Date    : 06/08/2013
Licence :
=========

Ce document intitulé « Fenetres déplacables et redimensionnable par la souris » issu de CommentCaMarche
(codes-sources.commentcamarche.net) est mis à disposition sous les termes de
la licence Creative Commons. Vous pouvez copier, modifier des copies de cette
source, dans les conditions fixées par la licence, tant que cette note
apparaît clairement.

Description :
=============

Voil&agrave; une petite source sans pr&eacute;tention, qui permet de cr&eacute;e
r une fenetre dans une page avec du contenu sp&eacute;cifique en dynamique.
<br
 />
<br />Le contenu peut &ecirc;tre du HTML en text (string) ou directement un
 objet.
<br />
<br />Pour plus d'info il suffit d'ouvrir le page html d'exempl
e et de regarder la fenetre avec l'image.
<br />
<br />La version finale march
e sous firefox et Internet Explorer.
<br />
<br />Le seul bug connu pour le mo
ment c'est, sous firefox, la zone de redimenssionement autour des contours de la
 fenetre est r&eacute;duire (je n'arrive pas &agrave; changer le curseur de body
 sous FF, voir ligne 543 de la source).
<br />
<br />Derni&egrave;re chose &ea
cute;tant donn&eacute; que le redimensionnement alourdit pas mal le code, j'ai l
aiss&eacute; une version sans dans le zip (d&eacute;placement uniquemen).
<br /
>
<br />Voil&agrave;, sinon comme d'habitude les commentaire (constructif bien 
s&ucirc;r) sont les bienvenues! Soyez indulgeants ce n'est que ma 2ieme vrai sou
rces en javascript.<h2>Source / Exemple :</h2>
<pre class='code' data-mode='java
script'>
///////////////
// COULEURS //
///////////////

//FENETRE
BCKGROUN
D		=	&quot;#FFFFFF&quot;;	//Couleur de fond de la fenetre
BORDER			=	&quot;#000
000&quot;;	//Couleur de la bordure

//BANDEAU
B_BCKGROUND		=	&quot;#6f6f6f&qu
ot;;	//Couleur du bandeau
B_TEXT			=	&quot;#FFFFFF&quot;;	//Couleur du titre


//BOUTTON &quot;FERMER&quot; &amp; &quot;REDUIRE&quot;
B_B_BCKGROUND	= 	&quot;
#FFFFFF&quot;;	//Couleur de Fond des bouttons &quot;Fermer&quot; et &quot;R&eacu
te;duire&quot;
B_B_BORDER		= 	&quot;#000000&quot;;	//Couleur de Bordure des bou
ttons &quot;Fermer&quot; et &quot;R&eacute;duire&quot;
B_B_TEXT		= 	&quot;#0000
00&quot;;	//Couleur de Texte des bouttons &quot;Fermer&quot; et &quot;R&eacute;d
uire&quot;

//BOUTTON &quot;FERMER&quot; &amp; &quot;REDUIRE&quot; ENFONCES
B
_BD_BCKGROUND	= 	&quot;#000000&quot;;	//Couleur de Fond des bouttons &quot;Ferme
r&quot; et &quot;R&eacute;duire&quot;
B_BD_BORDER		= 	&quot;#FFFFFF&quot;;	//Co
uleur de Bordure des bouttons &quot;Fermer&quot; et &quot;R&eacute;duire&quot;

B_BD_TEXT		= 	&quot;#FFFFFF&quot;;	//Couleur de Texte des bouttons &quot;Fermer&
quot; et &quot;R&eacute;duire&quot;

//SCROLLBAR
SBDARKSHADOW	=	&quot;#4f4f4f
&quot;;	//scrollbarDarkShadowColor
SB3DLIGHT 		=	&quot;#4f4f4f&quot;;	//scrollb
ar3dLightColor
SBARROW   		=	&quot;#FFFFFF&quot;;	//scrollbarArrowColor
SBBARB
ASE		=	&quot;#4f4f4f&quot;;	//scrollbarBaseColor
SBBARFACE		=	&quot;#8f8f8f&quo
t;;	//scrollbarFaceColor
SBHIGHTLIGHT	=	&quot;#FFFFFF&quot;;	//scrollbarHighlig
htColor
SBBARSHADOW		=	&quot;#8f8f8f&quot;;	//scrollbarShadowColor
SBBARTRACK	
	=	&quot;#FFFFFF&quot;;	//scrollbarTrackColor
	
//////////////
// OPTIONS //

//////////////

COMPORTEMENT_FERMETURE = &quot;cache&quot;; //Comportement en
 cas de clique sur le boutton &quot;Fermer&quot; ( 'cache' ou 'supprime' la fen&
ecirc;tre)
CONFIRME_FERMETURE = true; //Demande une confirmation ou non lors d'
un clique sur le boutton &quot;Fermer&quot;

COTES_REDIMENSIONNEMENT = &quot;a
ll&quot;; //Cot&eacute;s de la fenetre redimenssionables, &quot;classique&quot; 
= bas et droite, &quot;all&quot; = tous

//////////////////
// CONSTANTES //

//////////////////

//Constantes utilis&eacute;es pour cr&eacute;er un masque
  qui va nous permettre de savoir dans quelle(s) direction(s) le redimenssionnem
ent doit s'effectu&eacute;.
RES_UP 		= 1;	//Redimenssionement vers le haut
RES
_DOWN 	= 2;	//Redimenssionement vers le bas
RES_RIGHT	= 4;	//Redimenssionement 
vers la droite
RES_LEFT	= 8;	//Redimenssionement vers la gauche

RES_ZONE	= 8
;	//Zone de redimenssionement autour des borure ( D&eacute;faut : + ou - 5px)


MIN_HEIGHT 	= 75;	//Hauteur minimal pour une fenetre
MIN_WIDTH 	= 75;	//Largeu
r minimal pour une fenetre

///////////////
// GLOBALES //
///////////////


//Position du curseur de la souris (Mis &agrave; jour par le fonction appel&ea
cute; lors de l'evenement &quot;OnMouseMove&quot; de document)
curX = 0;
curY 
= 0;

resize_direction = 0; //Direction(s) du redimenssionement
in_resize = f
alse;

fenetre = null;	//La fenetre en cours de d&eacute;placement
fenetre_re
size = null;	//La fenetre en cours de redimmenssionnement

is_ie = ! (navigato
r.appName == &quot;Netscape&quot;); //Internet Explorer ou Netscape ?


/////
/////////////
// EVENEMENTS //
//////////////////

//Et on active l'event &q
uot;mousemove&quot; pour r&eacute;cup&eacute;rer les coordonn&eacute;e de la sou
ris au fur et &agrave; mesure
document.onmousemove = PdcDocumentOnMouseMove;
d
ocument.onmousedown = PdcDocumentOnMouseDown;
document.onmouseup = PdcDocumentO
nMouseUp;
 
//Cr&eacute;ation d'un fenetre
function CreerPdcFenetre( titre, l
argeur, hauteur, posLeft, posTop, contenu ){
		
	//On cr&eacute;er un objet &q
uot;Div&quot; correspondant &agrave; notre fenetre
	var fenetre = document.crea
teElement(&quot;div&quot;);   
	
	//Permet d'identifier une fenetre de fa&cced
il;on &agrave; ne pas &eacute;ffectuer d'action de redimensionnement sur un &eac
ute;l&eacute;ment qui n'est pas une fenetre
	
	fenetre.id = &quot;PdcFenetre&q
uot;;
	
	//On d&eacute;finit les propri&eacute;t&eacute;s de la fenetre
	
	/
/On d&eacute;finit la taille et on garde la taille par d&eacute;faut
	fenetre.m
y_width = ((is_ie ? 4 : 0) + largeur); //+ 4 pour la bordure si IE, 0 si FF
	fe
netre.style.width = fenetre.my_width +&quot;px&quot;; 
	
	fenetre.my_height = 
((is_ie ? 24 : 20) + hauteur); //+24 pour la bordure et le bandeau si IE, + 20 s
i FF
	fenetre.style.height = fenetre.my_height + &quot;px&quot;; 
	
	//Positi
on
	fenetre.style.position = &quot;absolute&quot;;
	fenetre.style.left = posLe
ft + &quot;px&quot;;
	fenetre.style.top = posTop + &quot;px&quot;;
	
	//Bordu
re
	fenetre.style.border = &quot;2px &quot; + BORDER + &quot; solid&quot;;
		

	//Couleur de fond pour avoir une fenetre non transparente
	fenetre.style.back
groundColor = BCKGROUND;
	
	//On cr&eacute;er notre bandeau
	bandeau = CreerP
dcBandeau( titre, fenetre, largeur);
	
	//On ajoute notre bandeau &agrave; la 
fentre
	fenetre.appendChild( bandeau);
	
	//On garde une r&eacute;f&eacute;re
nce &agrave; noter bandeau
	fenetre.bandeau = bandeau;
		
		
	//On cr&eacute
;er une div qui va accueillir notre contenu
	var div_contenu = document.createE
lement(&quot;div&quot;); 
	
	//On met &agrave; la taille demand&eacute;
	div_
contenu.style.width = largeur + &quot;px&quot;;
	div_contenu.style.height = hau
teur + &quot;px&quot;;
	
	//Dans le cas ou le contenu est plus grand que la di
v qui l'accueil, on affiche des scrollbar
	div_contenu.style.overflow = &quot;a
uto&quot;;
		
	//Mise au couleurs personnailis&eacute; des scrollbars
	div_co
ntenu.style.scrollbarDarkShadowColor	=	SBDARKSHADOW;
	div_contenu.style.scrollb
ar3dLightColor		=	SB3DLIGHT;
	div_contenu.style.scrollbarArrowColor		=	SBARROW;

	div_contenu.style.scrollbarBaseColor		=	SBBARBASE;
	div_contenu.style.scroll
barFaceColor		=	SBBARFACE;
	div_contenu.style.scrollbarHighlightColor	=	SBHIGHT
LIGHT;
	div_contenu.style.scrollbarShadowColor		=	SBBARSHADOW;
	div_contenu.st
yle.scrollbarTrackColor		=	SBBARTRACK;
	
	
	
	//On garde une r&eacute;f&eacu
te;rence &agrave; notre contenu pour la r&eacute;duction de la fenetre
	fenetre
.contenu = div_contenu; 
	
	//On garde &eacute;galement une r&eacute;ference d
ans l'autre sens (r&eacute;f&eacute;rence &agrave; la fenetre dans contenu)
	di
v_contenu.my_fenetre = fenetre;
		
	//Si on a du contenu on l'ajoute &agrave; 
la fenetre
	if(	contenu != null ){
	
		//Objet
		if( typeof( contenu) == &qu
ot;object&quot; ){
			div_contenu.appendChild( contenu);
			
		}
		//Ou stri
ng (par exemple : &quot;&lt;a href&quot;...&quot;&gt; .. &lt;/a&gt;&quot;
		els
e{
		
			tmp = document.createElement(&quot;div&quot;);
			tmp.innerHTML = co
ntenu;
			div_contenu.appendChild( tmp);			
		}
		
	}

	//On ajoute notre 
div &agrave; la fenetre
	fenetre.appendChild( div_contenu);
			
	//Par d&eacu
te;faut la fenetre est en dessous lorsque qu'une fenetre est d&eacute;plac&eacut
e;
	fenetre.style.zIndex = 1;

	//Met la fenetre au premier plan au moindre c
lique dessus.
	fenetre.onmousedown = pdcFenetreOnClick;
	
	//On ajoute notre 
fenetre &agrave; la page
	document.body.appendChild( fenetre);
		
	//et on re
tourne l'objet fenetre pour une utilisation &eacute;ventuelle (r&eacute;affichag
e apr&egrave;s fermeture par exemple)
	return fenetre;
}

//Fonction ind&eac
ute;pendante pour la cr&eacute;ation de l'objet bandeau
function CreerPdcBandea
u( titre, fenetre, largeur){

	// VOIR A LA FIN DU FICHIER POUR LE CODE HTML D
U BANDEAU
			
	var bandeau = document.createElement(&quot;table&quot;)
	
	ba
ndeau.style.width = largeur + &quot;px&quot;;
	bandeau.style.height = &quot;20p
x&quot;;
	bandeau.style.backgroundColor = B_BCKGROUND;
	bandeau.style.cursor =
 &quot;move&quot;;
	
	
	var bandeau_body = document.createElement(&quot;tbody
&quot;)
		
	//Notre unique ligne du tableau...	
	var tr = document.createElem
ent(&quot;tr&quot;);
	
	//On cr&eacute;er une case pour le texte du titre
	va
r td_titre = document.createElement(&quot;td&quot;);	
	td_titre.style.paddingTo
p = &quot;0px&quot;;
	td_titre.style.paddingBottom = &quot;0px&quot;;
	
	//On
 ajoute la gestion des &eacute;venements &agrave; notre bandeau
	
	//Lorsque l
'utilisateur appui sur le bouton de la souris
	td_titre.onmousedown = PdcFenetr
eOnMouseDown;	
	
	//Lorsque l'utilisateur relache le bouton de la souris
	td_
titre.onmouseup = PdcFenetreOnMouseUp;
	
	//R&eacute;duire / Agrandir la fenet
re en cas de double clique sur le bandeau
	td_titre.ondblclick  = PdcOnDoubleCl
ickReduire; 
	
	//On cr&eacute;er la Div qui va accueillir le titre
	var div 
= document.createElement(&quot;div&quot;)
	
	div.style.fontSize = &quot;12&quo
t;;
	div.style.width = (largeur - 36) + &quot;px&quot;; //Largeur de la fenetre
 moins la taille de nos 2 bouttons ( 12 + 12) moins la taille de la bordure (2 +
 2) moins la taille entre nos cellule (4 * 2) = .. 36 bien s&ucirc;r ! :p
	div.
style.overflow = &quot;hidden&quot;; //Ne pas agrandir la Div si le contenu est 
plus grand que sa taille
	div.style.color = B_TEXT;
	div.style.fontWeight = &q
uot;bold&quot;;
	div.style.whiteSpace = &quot;nowrap&quot;; //Pas de retour &ag
rave; la ligne
		
	
	//Le texte du titre
	div.innerHTML += titre;
	
		
	

	//On cr&eacute;er une 2i&egrave;me case pour notre tableau celle contenant le 
&quot;boutton&quot; pour r&eacute;duire la fenetre
	var td_reduire = document.c
reateElement(&quot;td&quot;);	
	
	td_reduire.style.padding = &quot;0px&quot;	

	td_reduire.style.backgroundColor = B_B_BCKGROUND;
	td_reduire.style.cursor = 
&quot;pointer&quot;;
	td_reduire.style.width = &quot;12px&quot;;
	td_reduire.s
tyle.height = &quot;12px&quot;;
	td_reduire.style.border = &quot;1px &quot; + B
_B_BORDER + &quot; solid&quot;;
	td_reduire.style.fontSize = &quot;10px&quot;;	

	td_reduire.style.fontWeight = &quot;bold&quot;;
	td_reduire.style.color = B_
B_TEXT;
	
	//On utilise les balise &lt;center&gt; car la propri&eacute;t&eacut
e; &quot;text-align&quot; ne fonctionne pas sous FF
	td_reduire.innerHTML = &qu
ot;&lt;center&gt;-&lt;/center&gt;&quot;;
		
	td_reduire.onmousedown = PdcBoutt
onOnMouseDown; //D&eacute;but du clique
	td_reduire.onmouseup = PdcBouttonOnMou
seUpReduire; //R&eacute;duction de la fenetre
	td_reduire.onmouseout = PdcBoutt
onOnMouseOut; //Si l'utilisateur commence son clique puis sort du &quot;boutton&
quot; pour annuler
		
		
	
	//On cr&eacute;er une 3i&egrave;me case pour not
re tableau celle contenant le &quot;boutton&quot; pour fermer la fenetre
	var t
d_fermer = document.createElement(&quot;td&quot;);	
	
	td_fermer.style.padding
 = &quot;0px&quot;	
	td_fermer.style.backgroundColor = B_B_BCKGROUND;
	td_ferm
er.style.cursor = &quot;pointer&quot;;
	td_fermer.style.width = &quot;12px&quot
;;
	td_fermer.style.height = &quot;12px&quot;;
	td_fermer.style.border = &quot
;1px &quot; + B_B_BORDER + &quot; solid&quot;;
	td_fermer.style.fontSize = &quo
t;10px&quot;;	
	td_fermer.style.fontWeight = &quot;bold&quot;;
	td_fermer.styl
e.color = B_B_TEXT;
	
	//On utilise les balise &lt;center&gt; car la propri&ea
cute;t&eacute; &quot;text-align&quot; ne fonctionne pas sous FF
	td_fermer.inne
rHTML = &quot;&lt;center&gt;X&lt;/center&gt;&quot;;
	
	td_fermer.onmousedown =
 PdcBouttonOnMouseDown; //D&eacute;but du clique
	td_fermer.onmouseup = PdcBout
tonOnMouseUpFermer; //Fermeture de la fenetre
	td_fermer.onmouseout = PdcBoutto
nOnMouseOut; //Si l'utilisateur commence son clique puis sort du &quot;boutton&q
uot; pour annuler
		
	//On Embo&icirc;te tout &ccedil;a....
		
	//On ajoute 
la div &agrave; notre case (TD)
	td_titre.appendChild( div);	
	//On ajoute la 
case &quot;titre&quot; &agrave; notre ligne
	tr.appendChild( td_titre);
	//On 
ajoute la case &quot;r&eacute;duire&quot; &agrave; notre ligne
	tr.appendChild(
 td_reduire);
	//On ajoute la case &quot;fermer&quot; &agrave; notre ligne
	tr
.appendChild( td_fermer);	
	//On ajoute notre ligne au corp du tableau	
	bande
au_body.appendChild( tr);	
	//On ajoute le corp au tableau au tableau
	bandeau
.appendChild( bandeau_body);
	
	
	//On ajoute une r&eacute;f&eacute;rence &ag
rave; la fenetre (l'objet parent) pour les 3 elements du bandeau
	
	//Permet d
'acc&egrave;der &agrave; la fenetre &agrave; partir des 3 &quot;objets&quot; du 
bandeau ('boutton' + titre)
	td_titre.my_fenetre = fenetre;
	td_fermer.my_fene
tre = fenetre;
	td_reduire.my_fenetre = fenetre;	

	bandeau.titre = div;
	

	return bandeau;
		
		
}

//Bandeau.OnMouseUp
function PdcFenetreOnMouseUp
(){	
		
	//On remet notre fonction par d&eacute;faut pour g&eacute;rer le d&ea
cute;placement
	this.onmousedown = PdcFenetreOnMouseDown;	
		
	//Effet de tra
nsparence
	if( is_ie )
		this.my_fenetre.style.filter = &quot;alpha(opacity=10
0);&quot;;	
	else
		this.my_fenetre.style.opacity = &quot;1&quot;;
	
	//Plus
 de d&eacute;placement de fenetre en cours
	fenetre = null;
}

//Bandeau.OnM
ouseDown
function PdcFenetreOnMouseDown(e){
		
	maj_coordonnees( e);
	
	//O
n garde les coordonn&eacute;e du pointeur par rapport &agrave; la fenetre
	this
.my_fenetre.CurLastX = curX - this.my_fenetre.offsetLeft;
	this.my_fenetre.CurL
astY = curY - this.my_fenetre.offsetTop;
		

	//On se met au dessus des autre
s fenetres lors du d&eacute;placement
	pdcSetFenetrePremierPlan( this.my_fenetr
e);
			
	//Effet de transparence
	if( is_ie )
		this.my_fenetre.style.filter
 = &quot;alpha(opacity=80);&quot;;	
	else
		this.my_fenetre.style.opacity = &q
uot;0.8&quot;;
	
	//On garde l'objet fenetre (du bandeau) qui correspond &agra
ve; la fenetre pour laquelle on g&egrave;re le d&eacute;placement
	fenetre = th
is.my_fenetre;
	
	//Annulation de l'evenement pour &eacute;viter le surligneme
nt
	return false;
}


//Fonction qui met &agrave; jour les coordonn&eacute;
es de la souris en fonction du type de navigateur
function maj_coordonnees(e){

	
	if( is_ie ){
		curX = event.clientX;
		curY = event.clientY;
	}
	else{

		curY = e.clientY;
		curX = e.clientX;
	}
}


//Enfoncement des boutton 
&quot;r&eacute;duire&quot; et &quot;fermer&quot;
function PdcBouttonOnMouseDown
(){
				
	this.style.border = &quot;1px &quot; + B_BD_BORDER + &quot; inset&qu
ot;;	
	
	this.style.backgroundColor = B_BD_BCKGROUND;
	this.style.color = B_B
D_TEXT;
	
	//Annulation de l'evenement pour &eacute;viter le surlignement
	re
turn false;
}

//Relache la souris sur le boutton &quot;r&eacute;duire&quot; 
(On r&eacute;duit la fenetre)
function PdcBouttonOnMouseUpReduire(){
	
	
	th
is.style.border = &quot;1px &quot; + B_B_BORDER + &quot; solid&quot;;	
	
	this
.style.backgroundColor = B_B_BCKGROUND;
	this.style.color = B_B_TEXT;
	
	if( 
this.my_fenetre.contenu.style.display == &quot;none&quot; ){
		this.my_fenetre.
contenu.style.display = &quot;block&quot;; //On r&eacute;affiche le contenu					
	
		this.my_fenetre.style.height = this.my_fenetre.my_height + &quot;px&quot;; 
//et on met &agrave; jour la taille de la fenetre				
	}
	else{
		this.my_fen
etre.contenu.style.display = &quot;none&quot;;	//On cache le contenu
		this.my_
fenetre.style.height = ( is_ie ? 24 : 20) + &quot;px&quot;; //Taille de la fenet
re = taille du bandeau + bordure
	}
	
}

//Double click sur le bandeau (tit
re) (On r&eacute;duit la fenetre)
function PdcOnDoubleClickReduire(){
			
	if
( this.my_fenetre.contenu.style.display == &quot;none&quot; ){
		this.my_fenetr
e.contenu.style.display = &quot;block&quot;; //On r&eacute;affiche le contenu			
			
		this.my_fenetre.style.height = this.my_fenetre.my_height + &quot;px&quot;
; //et on met &agrave; jour la taille de la fenetre				
	}
	else{
		this.my_f
enetre.contenu.style.display = &quot;none&quot;;	//On cache le contenu
		this.m
y_fenetre.style.height = ( is_ie ? 24 : 20) + &quot;px&quot;; //Taille de la fen
etre = taille du bandeau + bordure
	}
	
	//Annulation de l'evenement pour &ea
cute;viter la selection
	return false;
}

//Relache la souris sur le boutton
 &quot;fermer&quot; (On ferme ou cache la fenetre)
function PdcBouttonOnMouseUp
Fermer(){
	
	this.style.border = &quot;1px &quot; + B_B_BORDER + &quot; solid&
quot;;	
	
	this.style.backgroundColor = B_B_BCKGROUND;
	this.style.color = B_
B_TEXT;
	
	//Confirmation avant fermeture?
	if( CONFIRME_FERMETURE )
		if( !
confirm(&quot;Etes-vous s&ucirc;r de vouloir fermer la fen&ecirc;tre?&quot;) )

			return;
	
	
	//Lors de la demande de fermeture soit on cache la fenetre
	
if( COMPORTEMENT_FERMETURE == &quot;cache&quot;){
		this.my_fenetre.style.displ
ay = &quot;none&quot;;
	}
	//Soit on l'enl&egrave;ve d&eacute;finitivement de 
notre page
	else{
		document.body.removeChild( this.my_fenetre);
	}
}

//s
orti des boutton &quot;r&eacute;duire&quot; et &quot;fermer&quot; pour la souris

function PdcBouttonOnMouseOut(){
		
	this.style.border = &quot;1px &quot; + 
B_B_BORDER + &quot; solid&quot;;	
	
	this.style.backgroundColor = B_B_BCKGROUN
D;
	this.style.color = B_B_TEXT;	
}

//Document.OnMouseMove
function PdcDoc
umentOnMouseMove(e){
				
	//D&eacute;placement
	if( fenetre != null ){
	
	
	maj_coordonnees(e);
		
		//On bloque le d&eacute;placement en dehors de la fe
netre du navigateur
		if( curX - fenetre.CurLastX &lt; 0 || curY -fenetre.CurLa
stY &lt; 0 )
			return;
			
		//D&eacute;place la fenetre en fonction de la p
osition actuelle du curseur et de la position du curseur par rapport &agrave; la
 fenetre
		fenetre.style.left = curX -fenetre.CurLastX + &quot;px&quot;;
		fen
etre.style.top = curY - fenetre.CurLastY + &quot;px&quot;;	
	}
	//Redimenssion
nement
	else{
		
		
		if( fenetre_resize == null ){
		
			//On cherche &ag
rave; savoir si la souris est sur un element de Type &quot;Fenetre&quot; (class 
= &quot;PdcFenetre&quot;)			
			if( is_ie ){
				if( event.srcElement.id != &q
uot;PdcFenetre&quot; )				
					return;									
			}
			else{
				
				if(
 e.target.id != &quot;PdcFenetre&quot;)
					return;						
			}
						
			//
La fenetre &agrave; redimenssioner
			if( is_ie )
				fenetre_resize = event.s
rcElement;
			else
				fenetre_resize = e.target;			
		}
				
		//On r&eacu
te;cup&egrave;re la position de la souris par rapport &agrave; la fenetre
		if(
 is_ie ){						
			posX = event.clientX - fenetre_resize.offsetLeft;			
			pos
Y = event.clientY - fenetre_resize.offsetTop;
			eX = event.clientX;
			eY = e
vent.clientY;
		}
		else{			
			posX = e.clientX - fenetre_resize.offsetLeft;
			
			posY = e.clientY - fenetre_resize.offsetTop;
			eX = e.clientX;
			eY 
= e.clientY;
		}
		
		//Pas de redimenssionnement en cours, on cherche &agrav
e; savoir dans quelle zone (direction) la souris se trouve
		if( ! in_resize ){

			
			//Aucune direction par d&eacute;faut
			resize_direction = 0;
			
	
		//Type de curseur
			resize_cursor = &quot;&quot;;
			
			//On cherche &agr
ave; savoir dans quelle(s) direction(s) doit on redimenssioner	
			if ( COTES_R
EDIMENSIONNEMENT == &quot;all&quot; &amp;&amp; Math.abs(posY) &lt; RES_ZONE){
	
			resize_direction += RES_UP;
				resize_cursor += &quot;n&quot;;
			}
			el
se if( posY &gt; (fenetre_resize.offsetHeight - RES_ZONE) &amp;&amp; posY &lt; (
fenetre_resize.offsetHeight + RES_ZONE) ){
				resize_direction += RES_DOWN;
	
			resize_cursor += &quot;s&quot;;
			}
			
			if( COTES_REDIMENSIONNEMENT ==
 &quot;all&quot; &amp;&amp; Math.abs(posX) &lt; RES_ZONE ){
				resize_directio
n += RES_LEFT;
				resize_cursor += &quot;w&quot;;
			}
			else if( posX &gt;
 (fenetre_resize.offsetWidth - RES_ZONE) &amp;&amp; posX &lt; (fenetre_resize.of
fsetWidth + RES_ZONE) ){
				resize_direction += RES_RIGHT;
				resize_cursor 
+= &quot;e&quot;;			
			}
				
					

			//Si on est sorti de la zone de re
dimenssionement, on remet le curseur par d&eacute;faut
			if( resize_direction 
== 0 ){
				
				fenetre_resize.style.cursor = &quot;default&quot;;		
				doc
ument.body.style.cursor = &quot;defaut&quot;;			
				fenetre_resize = null;
		
		
				return;
			}
			//Sinon on met le curseur qui va bien
			else{
				

								
				fenetre_resize.style.cursor = resize_cursor + &quot;-resize&quot;
;					
				//HS sous FF !
				document.body.style.cursor = resize_cursor + &qu
ot;-resize;&quot;;				
			}
			
			return;		
		}		
		//En cours de redimens
sionement
		else{
		
		
 
						
			if( resize_direction &amp; RES_RIGHT )
{
				
				if( posX &gt;= MIN_WIDTH){
					
					//Redimensionne la fenetre

					fenetre_resize.style.width = posX + &quot;px&quot;;
					//Redimensionne 
le contenu
					fenetre_resize.contenu.style.width = (posX - (is_ie ? 4 : 0) ) 
+ &quot;px&quot;;					
					//Redimensionne le bandeau
					fenetre_resize.ban
deau.style.width = (posX - (is_ie ? 4 : 0) ) + &quot;px&quot;;										
					/
/Puis la div contenant le titre (taille fixe pour gestion de l'overflow)
					f
enetre_resize.bandeau.titre.style.width = (posX - (is_ie ? 40 : 36) ) + &quot;px
&quot;;										
					
					//On garde en &quot;m&eacute;moire&quot; la nouve
lle taille de la fenetre pour la r&eacute;duction/agrandissement
					fenetre_r
esize.my_width = posX;
				}
			}
			
			if( resize_direction &amp; RES_LEFT
 ){
				
				new_width = (fenetre_resize.offsetWidth - 4 - posX );
				
				
if( new_width &gt;= MIN_WIDTH){
					
					//Redimensionne le contenu
					fe
netre_resize.contenu.style.width = new_width + &quot;px&quot;;					
					//Redi
mensionne le bandeau
					fenetre_resize.bandeau.style.width = new_width + &quo
t;px&quot;;										
					//Puis la div contenant le titre (taille fixe pour g
estion de l'overflow)
					fenetre_resize.bandeau.titre.style.width = (new_widt
h - 36 ) + &quot;px&quot;;	
					
					//D&eacute;place + Redimensionne la fen
etre
					fenetre_resize.style.left = eX + &quot;px&quot;;				
					fenetre_re
size.style.width =  new_width + &quot;px&quot;;
					
					//On garde en &quot
;m&eacute;moire&quot; la nouvelle taille de la fenetre pour la r&eacute;duction/
agrandissement
					fenetre_resize.my_width = posX;
										
				}
				//L
orsqu'on &agrave; atteint la taillle minimal, on se contente de d&eacute;placer 
la fenetre
				else{
				
					//D&eacute;place la fenetre
					fenetre_resi
ze.style.left = eX + &quot;px&quot;;				
				}
			}		
			
			if( resize_dire
ction &amp; RES_UP ){
				
				new_height = (fenetre_resize.offsetHeight - 4 -
 posY );
				
				if( new_height &gt;= MIN_HEIGHT){
					
					//Redimenssio
nne le contenu
					fenetre_resize.contenu.style.height = (new_height - 20 ) + 
&quot;px&quot;;
					
					//Redimenssionne et d&eacute;place la fenetre
				
	fenetre_resize.style.top = eY + &quot;px&quot;;
					fenetre_resize.style.heig
ht =  new_height + &quot;px&quot;;	

					//Si l'utilisateur redimenssione ver
s le bas et que la fenetre et en mode &quot;r&eacute;duite&quot; on r&eacute;aff
iche le contenu
					if( fenetre_resize.contenu.style.display == &quot;none&quo
t; )
						fenetre_resize.contenu.style.display = &quot;block&quot;; 					
			
			
					//On garde en &quot;m&eacute;moire&quot; la nouvelle taille de la fene
tre pour la r&eacute;duction/agrandissement
					fenetre_resize.my_height = pos
Y;
					
				}
				//Lorsqu'on &agrave; atteint la taillle minimal, on se con
tente de d&eacute;placer la fenetre
				else{
				
					//D&eacute;place la f
enetre
					fenetre_resize.style.top = eY + &quot;px&quot;;				
				}
				
	
			
			}
			
			if( resize_direction &amp; RES_DOWN ){			
				
				if( posY
 &gt;= MIN_HEIGHT){	
					//Redimenssionne la fenetre
					fenetre_resize.styl
e.height = posY + &quot;px&quot;;					
					//Redimenssionne le contenu
					f
enetre_resize.contenu.style.height = (posY - (is_ie ? 24 : 20) ) + &quot;px&quot
;;	

					//Si l'utilisateur redimenssione vers le bas et que la fenetre et en
 mode &quot;r&eacute;duite&quot; on r&eacute;affiche le contenu
					if( fenetr
e_resize.contenu.style.display == &quot;none&quot; )
						fenetre_resize.conte
nu.style.display = &quot;block&quot;; 					
						
					//On garde en &quot;m&
eacute;moire&quot; la nouvelle taille de la fenetre pour la r&eacute;duction/agr
andissement
					fenetre_resize.my_height = posY;
				}
				
			}			
		}			

	}
	
	//Annulation de l'evenement pour &eacute;viter le surlignement
	retur
n false;
}


//Souris enfonc&eacute;e sur Document (D&eacute;but de redimens
sionnement...?)
function PdcDocumentOnMouseDown(e){	
	in_resize = true;
	//On
 se met au dessus des autres fenetres lors du redimenssionement
	if( fenetre_re
size != null )
		pdcSetFenetrePremierPlan( fenetre_resize);
	
}


//Souris
 rel&acirc;ch&eacute;e sur Document (Fin de redimenssionnement)
function PdcDoc
umentOnMouseUp(e){
	in_resize = false;		
}

//Met la fenetre au premier plan
 lorsqu'on clique dessus
function pdcFenetreOnClick(){
	pdcSetFenetrePremierPl
an( this);
}

//Permet de mettre une fenetre en premier plan par rapport au a
utres.
function pdcSetFenetrePremierPlan( fenetre ){
			
	//Pour IE on travai
l sur : document.body.children
	//Pour FF on travail sur : document.body.childN
odes
	//my_child = ( is_ie ? document.body.children : document.body.childNodes 
);
	
	
	my_child = document.body.childNodes;
	
	//On met tout les objet de 
type &quot;Fenetre&quot; en zIndex=1 (par d&eacute;faut) et celle d&eacute;mand&
eacute; en zIndex=2	
	for( var i=0; i &lt; my_child.length; i++){
		if( my_chi
ld[i].id == &quot;PdcFenetre&quot; )
			my_child[i].style.zIndex = 1;
	}
		

	fenetre.style.zIndex = 2;
}

/*

CODE HTML + STYLE POUR LE BANDEAU

&lt;
table style=&quot;width: 100px; background-Color: #6f6f6f; cursor: move;&quot;&g
t;	
&lt;tr&gt;


	&lt;td style=&quot;padding-top: 0px; padding-bottom: 0px;&
quot;&gt;
		&lt;div align=&quot;left&quot; style=&quot;font-size: 12; width: 50
px; overflow: hidden; color: #FFFFFF; font-weight:bold; white-space:nowrap;&quot
;&gt;Mon super titre&lt;/div&gt;
	&lt;/td&gt;
	
	&lt;td style=&quot;padding:0
px; background-Color: #FFFFFF; cursor: pointer; width:12px; height:12px; border:
1px #000000 solid; font-size:10; text-align= center; font-weight:bold;&quot;&gt;
&lt;center&gt;-&lt;/center&gt;&lt;/td&gt;

	&lt;td style=&quot;padding:0px; ba
ckground-Color: #FFFFFF; cursor: pointer; width:12px; height:12px; border:1px #0
00000 solid; font-size:10; text-align= center; font-weight:bold;&quot;&gt;&lt;ce
nter&gt;X&lt;/center&gt;&lt;/td&gt;
			
&lt;/tr&gt;
&lt;/table&gt;


*/</p
re><h2> Conclusion : </h2>Voil&agrave; la version finale de ce petit projet Java
script. Les fenetres sont d&eacute;pla&ccedil;ables et redimenssionnables comme 
pr&eacute;vu.
