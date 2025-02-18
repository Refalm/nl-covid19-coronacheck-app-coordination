
# Partial disclosure identity

As stated in the security framework document; it is highly desirable that people cannot
exchange certificates too easily. Of the various methods to prevent this (personalia,
passport photo, etc) the tradeoff made in that document calls for the partial disclosure
of some, but not all, of the data contained in the date of birth, first name and fammily
name. As to allow these to be compared to other data. Such as an identiy card.

However in order to allow the verifier to trust this data - it needs to be in the signed
part of the statement. As the combination of name and date of birth is rather unique, this
will, for most people, break the unlinkability. It makes people uniquely indentifyable
by their data - as they move throuch society.

For this reason *less* data will be used. I.e. just the initial of the first name and, for
example, the month (but not year or day) of birth.

## Tradeoff between security and privacy

For this a tradeoff needs to be made between security and privacy

* Include too much about the person in the Qr code - and they become trackable. Thus 
loosing security.

* Include too little - and it becomes easy to find someone in your immediate social circle
 with whom you can swap a phone.

Note that the vaccination, test or recvery data also contains an issue/expery/time indication. These also
introduce a level of entropy (e.g. if the test cert is 48 hours valid - and the time granualrity is
2 hours -- then an additional 24 permutations are to be added (assuming 24x7 issuing, propalby less)).

These permutations affect the privacy -within- the validity horizon of the certificate. While the other 
data is expescially relevant for identifcatin across certificates. So it is crucial for long lived
certificates to keep these numbers low; less so for very short lived certificates.

## Target

For this effort - two competing targets are to be met.

1. You need to ask several hundred people (333) in order to find someone in your social 
circle with a 'match'. Somewhat taking into account the fact that family names co-incide 
slightly more in your social circle.

1. If a 1000 people are scanned - there should be roughly a 50/50 chance that there is at least one other person with the same 'credentials' as 


## Calculation

On order to find the right strategy - a table with all first letters of the first name and the family name and their incidence is taken.

Note that common prefixes; such as the Dutch 'van, de, etc' are all *removed* prior to taking the first letter.

From here we try several strategies:

* show First initial First name (V) -- so 27 permutations. Some common (J, nearly 13% of the population), some rare (e.g. Q and X).
* show First initial Family name (F) -- again 27 permutations
* show Both (V+F) -- 27x27 permutations. Some common (e.g. J.B. nearly 2 percent of society)
* combine the above with month (1..12) or day(1..31) of birth.
* combine the above month (1..12) *and* day(1..31) of birth.

## Rule

And for each calculate the percentage with that permutation. Any that is within the range set above (0.1% .. 0.3%) is considered acceptable.

Of these we take the one that is:

1. Nearest to the middle of that range
2. If possible does *not* contain a family name (as these are likely to dominate in the social circle).

And if these rules do not yield a solution (for about 3%) of the population - we pick the one nearest to the middle of that range regardless.

## Rule to apply

During issuing - the backend will lookup the V and F in the table; and then include the fields specified in the signed data.

### Example

So for someone called *Albert Akkersloot, Born on the 12 of March 1990* - the rule is *FM*. So included are "V=A" and "M=March" while 'F' and 'D' are left empty.

# Full table

```
Version: 1.02

At least 1000 for a fifty fifty change that you share the data shown:

RISK_PRIVACY= 0.1 # Percent

At least 333 people in your vicintiy for a fifty fifty chance that you can 
borrow a phone (not quite true - as we in an 'F' there are a lot of people who
have the same 'F' -- so perhaps we should; given more options; go light on 
the 'F' and pick 'V' instead if roughly equal).

RISK_FRAUD  = 0.3; # Percent - 

Note:  this does not yet reflect the 'V' issue of the 'Van somethings'.

Pair	selected	best	sans-F		qualifying
AA	FM	 # FM			FM VF
AB	VFM	 # VFM			VFM
AC	VF	 # VF			VF FM
AD	FD	 # FD			FD
AE	FM	 # FM			FM VF
AF	VF	 # VF			VF FM
AG	FD	 # FD			FD
AH	FD	 # FD			FD
AI	VD	 # VD*			
AJ	FM	 # FM			FM VF
AK	FD	 # FD			FD
AL	FD	 # FD			FD
AM	FD	 # FD			FD
AN	VF	 # VF			VF FM
AO	VF	 # VF			VF FM
AP	FD	 # FD			FD
AQ	VD	 # VD*			
AR	FD	 # FD			FD
AS	FD	 # FD*			
AT	FM	 # FM			FM VF
AU	F	 # F			F
AV	FD	 # FD			FD
AW	FD	 # FD			FD
AX	VD	 # VD*			
AY	F	 # F			F
AZ	VF	 # VF			VF FM
BA	VM	 # FM	VM		FM VM VD
BB	VM	 # VM	VM		VM VD
BC	VM	 # VM	VM		VM FM VD
BD	VM	 # FD	VM		FD VF VM VD
BE	VM	 # FM	VM		FM VM VD
BF	VM	 # VM	VM		VM FM VD
BG	VM	 # FD	VM		FD VM VF VD
BH	VM	 # VM	VM		VM VF FD VD
BI	VM	 # VM	VM		VM VD
BJ	VM	 # FM	VM		FM VM VD
BK	VM	 # VM	VM		VM FD VF VD
BL	VM	 # FD	VM		FD VF VM VD
BM	VM	 # FD	VM		FD VF VM VD
BN	VM	 # FM	VM		FM VM VD
BO	VM	 # FM	VM		FM VM VD
BP	VM	 # VM	VM		VM FD VF VD
BQ	VM	 # VM	VM		VM VD
BR	VM	 # FD	VM		FD VF VM VD
BS	VM	 # VM	VM		VM VF VD
BT	VM	 # FM	VM		FM VM VD
BU	VM	 # VM	VM		VM F VD
BV	VM	 # VF	VM		VF FD VM VD
BW	VM	 # VF	VM		VF FD VM VD
BX	VM	 # VM	VM		VM VD
BY	VM	 # F	VM		F VM VD
BZ	VM	 # VM	VM		VM FM VD
CA	VD	 # FM	VD		FM VD VF
CB	VD	 # VD	VD		VD
CC	VD	 # VD	VD		VD FM
CD	VD	 # VD	VD		VD FD VF
CE	VD	 # FM	VD		FM VD VF
CF	VD	 # VD	VD		VD FM
CG	VD	 # VF	VD		VF VD FD
CH	VD	 # VD	VD		VD FD
CI	VD	 # VD	VD		VD
CJ	VD	 # VD	VD		VD FM VF
CK	VD	 # VD	VD		VD FD
CL	VD	 # VD	VD		VD VF FD
CM	VD	 # FD	VD		FD VD VF
CN	VD	 # FM	VD		FM VD
CO	VD	 # FM	VD		FM VD VF
CP	VD	 # VF	VD		VF VD FD
CQ	VD	 # VD	VD		VD
CR	VD	 # VF	VD		VF VD FD
CS	VD	 # VD	VD		VD
CT	VD	 # FM	VD		FM VD VF
CU	VD	 # VD	VD		VD F
CV	VD	 # FD	VD		FD VD
CW	VD	 # FD	VD		FD VD VF
CX	VD	 # VD	VD		VD
CY	VD	 # VD	VD		VD F
CZ	VD	 # VD	VD		VD FM
DA	VD	 # FM	VD		FM VD
DB	VD	 # VD	VD		VD
DC	VD	 # FM	VD		FM VD
DD	VD	 # VF	VD		VF FD VD
DE	VD	 # FM	VD		FM VD
DF	VD	 # VD	VD		VD FM
DG	VD	 # VF	VD		VF FD VD
DH	VD	 # VD	VD		VD FD
DI	VD	 # VD	VD		VD
DJ	VD	 # FM	VD		FM VD VF
DK	VD	 # FD	VD		FD VD
DL	VD	 # VF	VD		VF FD VD
DM	VD	 # VF	VD		VF FD VD
DN	VD	 # FM	VD		FM VD
DO	VD	 # FM	VD		FM VD
DP	VD	 # VF	VD		VF FD VD
DQ	VD	 # VD	VD		VD
DR	VD	 # VF	VD		VF FD VD
DS	VD	 # VD	VD		VD
DT	VD	 # FM	VD		FM VD VF
DU	VD	 # VD	VD		VD F
DV	VD	 # FD	VD		FD VD VF
DW	VD	 # VF	VD		VF FD VD
DX	VD	 # VD	VD		VD
DY	VD	 # F	VD		F VD
DZ	VD	 # FM	VD		FM VD
EA	VD	 # FM	VD		FM VD VF
EB	VD	 # VD	VD		VD
EC	VD	 # VD	VD		VD FM
ED	VD	 # VF	VD		VF FD VD
EE	VD	 # FM	VD		FM VD VF
EF	VD	 # VD	VD		VD FM
EG	VD	 # VF	VD		VF VD FD
EH	VD	 # VD	VD		VD FD
EI	VD	 # VD	VD		VD
EJ	VD	 # FM	VD		FM VD VF
EK	VD	 # VD	VD		VD FD
EL	VD	 # VF	VD		VF FD VD
EM	VD	 # FD	VD		FD VD VF
EN	VD	 # FM	VD		FM VD
EO	VD	 # FM	VD		FM VD
EP	VD	 # VF	VD		VF VD FD
EQ	VD	 # VD	VD		VD
ER	VD	 # VF	VD		VF VD FD
ES	VD	 # VD	VD		VD
ET	VD	 # FM	VD		FM VD VF
EU	VD	 # VD	VD		VD F
EV	VD	 # FD	VD		FD VD
EW	VD	 # FD	VD		FD VD VF
EX	VD	 # VD	VD		VD
EY	VD	 # F	VD		F VD
EZ	VD	 # VD	VD		VD FM
FA	VM	 # FM	VM		FM VM
FB	VM	 # VM	VM		VM
FC	VM	 # VM	VM		VM FM
FD	VM	 # VM	VM		VM FD VF
FE	VM	 # FM	VM		FM VM
FF	VM	 # VM	VM		VM FM
FG	VM	 # VM	VM		VM FD VF
FH	VM	 # VM	VM		VM VF FD
FI	VM	 # VM	VM		VM
FJ	VM	 # VM	VM		VM FM
FK	VM	 # VF	VM		VF VM FD
FL	VM	 # VM	VM		VM FD VF
FM	VM	 # FD	VM		FD VM VF
FN	VM	 # FM	VM		FM VM
FO	VM	 # FM	VM		FM VM
FP	VM	 # VM	VM		VM FD VF
FQ	VM	 # VM	VM		VM
FR	VM	 # VM	VM		VM FD VF
FS	VM	 # VM	VM		VM VF
FT	VM	 # FM	VM		FM VM
FU	VM	 # VM	VM		VM F
FV	VM	 # VF	VM		VF FD VM
FW	VM	 # FD	VM		FD VM VF
FX	VM	 # VM	VM		VM
FY	VM	 # VM	VM		VM F
FZ	VM	 # VM	VM		VM FM
GA	VD	 # FM	VD		FM VD
GB	VD	 # VD	VD		VD
GC	VD	 # FM	VD		FM VD
GD	VD	 # VF	VD		VF FD VD
GE	VD	 # FM	VD		FM VD
GF	VD	 # VD	VD		VD FM
GG	VD	 # VF	VD		VF FD VD
GH	VD	 # VD	VD		VD FD
GI	VD	 # VD	VD		VD
GJ	VD	 # FM	VD		FM VD VF
GK	VD	 # VD	VD		VD FD
GL	VD	 # VF	VD		VF FD VD
GM	VD	 # VF	VD		VF FD VD
GN	VD	 # FM	VD		FM VD
GO	VD	 # FM	VD		FM VD
GP	VD	 # VF	VD		VF VD FD
GQ	VD	 # VD	VD		VD
GR	VD	 # VF	VD		VF FD VD
GS	VD	 # VD	VD		VD
GT	VD	 # FM	VD		FM VD VF
GU	VD	 # VD	VD		VD F
GV	VD	 # FD	VD		FD VD VF
GW	VD	 # FD	VD		FD VF VD
GX	VD	 # VD	VD		VD
GY	VD	 # F	VD		F VD
GZ	VD	 # FM	VD		FM VD
HA	VD	 # FM	VD		FM VD VF
HB	VD	 # VD	VD		VD
HC	VD	 # VD	VD		VD FM
HD	VD	 # VD	VD		VD FD VF
HE	VD	 # FM	VD		FM VD VF
HF	VD	 # VD	VD		VD FM
HG	VD	 # VF	VD		VF VD FD
HH	VD	 # VD	VD		VD FD
HI	VD	 # VD	VD		VD
HJ	VD	 # VD	VD		VD VF FM
HK	VD	 # VD	VD		VD FD
HL	VD	 # VD	VD		VD VF FD
HM	VD	 # FD	VD		FD VD VF
HN	VD	 # VD	VD		VD FM VF
HO	VD	 # FM	VD		FM VD VF
HP	VD	 # VF	VD		VF VD FD
HQ	VD	 # VD	VD		VD
HR	VD	 # VF	VD		VF VD FD
HS	VD	 # VD	VD		VD
HT	VD	 # FM	VD		FM VD VF
HU	VD	 # VD	VD		VD F
HV	VD	 # VD	VD		VD FD
HW	VD	 # FD	VD		FD VD VF
HX	VD	 # VD	VD		VD
HY	VD	 # VD	VD		VD F
HZ	VD	 # VD	VD		VD FM
IA	VM	 # FM	VM		FM VM
IB	VM	 # VM	VM		VM VF
IC	VM	 # VM	VM		VM FM
ID	VM	 # FD	VM		FD VM
IE	VM	 # FM	VM		FM VM
IF	VM	 # VM	VM		VM FM
IG	VM	 # VM	VM		VM FD
IH	VM	 # VF	VM		VF VM FD
II	VM	 # VM	VM		VM
IJ	VM	 # VM	VM		VM FM
IK	VM	 # VM	VM		VM VF FD
IL	VM	 # VM	VM		VM FD
IM	VM	 # FD	VM		FD VM VF
IN	VM	 # FM	VM		FM VM
IO	VM	 # FM	VM		FM VM
IP	VM	 # VM	VM		VM FD
IQ	VM	 # VM	VM		VM
IR	VM	 # VM	VM		VM FD
IS	VM	 # VF	VM		VF VM
IT	VM	 # FM	VM		FM VM
IU	VM	 # VM	VM		VM F
IV	VM	 # FD	VM		FD VM VF
IW	VM	 # FD	VM		FD VM
IX	VM	 # VM	VM		VM
IY	VM	 # VM	VM		VM F
IZ	VM	 # VM	VM		VM FM
JA	FM	 # FM			FM VF
JB	VFM	 # VFM			VFM
JC	VF	 # VF			VF FM
JD	FD	 # FD			FD
JE	FM	 # FM			FM
JF	VF	 # VF			VF FM
JG	FD	 # FD			FD
JH	FD	 # FD			FD VFM
JI	VF	 # VF*			
JJ	FM	 # FM			FM
JK	FD	 # FD			FD
JL	FD	 # FD			FD
JM	FD	 # FD			FD
JN	FM	 # FM			FM VF
JO	FM	 # FM			FM VF
JP	FD	 # FD			FD
JQ	F	 # F*			
JR	FD	 # FD			FD
JS	VFM	 # VFM			VFM
JT	FM	 # FM			FM
JU	F	 # F			F
JV	FD	 # FD			FD
JW	FD	 # FD			FD
JX	VMD	 # VMD*			
JY	F	 # F			F
JZ	VF	 # VF			VF FM
KA	VM	 # FM	VM		FM VM
KB	VM	 # VM	VM		VM VF
KC	VM	 # VM	VM		VM FM
KD	VM	 # VM	VM		VM FD VF
KE	VM	 # FM	VM		FM VM
KF	VM	 # VM	VM		VM FM
KG	VM	 # VM	VM		VM FD
KH	VM	 # VF	VM		VF VM FD
KI	VM	 # VM	VM		VM
KJ	VM	 # VM	VM		VM FM
KK	VM	 # VF	VM		VF VM FD
KL	VM	 # VM	VM		VM FD VF
KM	VM	 # VM	VM		VM FD VF
KN	VM	 # VM	VM		VM FM
KO	VM	 # VM	VM		VM FM
KP	VM	 # VM	VM		VM FD
KQ	VM	 # VM	VM		VM
KR	VM	 # VM	VM		VM FD VF
KS	VM	 # VF	VM		VF VM
KT	VM	 # VM	VM		VM FM
KU	VM	 # VM	VM		VM F
KV	VM	 # VM	VM		VM FD VF
KW	VM	 # VM	VM		VM FD VF
KX	VM	 # VM	VM		VM
KY	VM	 # VM	VM		VM F
KZ	VM	 # VM	VM		VM FM
LA	VD	 # FM	VD		FM VD VF
LB	VD	 # VD	VD		VD
LC	VD	 # VD	VD		VD FM
LD	VD	 # VF	VD		VF FD VD
LE	VD	 # FM	VD		FM VD VF
LF	VD	 # VD	VD		VD FM
LG	VD	 # VF	VD		VF FD VD
LH	VD	 # VD	VD		VD FD
LI	VD	 # VD	VD		VD
LJ	VD	 # FM	VD		FM VF VD
LK	VD	 # VD	VD		VD FD
LL	VD	 # VF	VD		VF FD VD
LM	VD	 # FD	VD		FD VD VF
LN	VD	 # FM	VD		FM VD
LO	VD	 # FM	VD		FM VD VF
LP	VD	 # VF	VD		VF VD FD
LQ	VD	 # VD	VD		VD
LR	VD	 # VF	VD		VF FD VD
LS	VD	 # VD	VD		VD
LT	VD	 # FM	VD		FM VD VF
LU	VD	 # VD	VD		VD F
LV	VD	 # FD	VD		FD VD
LW	VD	 # FD	VD		FD VD VF
LX	VD	 # VD	VD		VD
LY	VD	 # F	VD		F VD
LZ	VD	 # VD	VD		VD FM
MA	FM	 # FM			FM
MB	VFM	 # VFM			VFM
MC	VF	 # VF			VF FM
MD	FD	 # FD			FD
ME	FM	 # FM			FM VF
MF	VF	 # VF			VF FM
MG	FD	 # FD			FD
MH	FD	 # FD			FD
MI	VF	 # VF*			
MJ	FM	 # FM			FM
MK	FD	 # FD			FD
ML	FD	 # FD			FD
MM	FD	 # FD			FD
MN	FM	 # FM			FM VF
MO	FM	 # FM			FM VF
MP	FD	 # FD			FD
MQ	F	 # F*			
MR	FD	 # FD			FD
MS	FD	 # FD*			
MT	FM	 # FM			FM
MU	F	 # F			F
MV	FD	 # FD			FD
MW	FD	 # FD			FD
MX	VMD	 # VMD*			
MY	F	 # F			F
MZ	VF	 # VF			VF FM
NA	VM	 # FM	VM		FM VM
NB	VM	 # VM	VM		VM
NC	VM	 # VM	VM		VM FM
ND	VM	 # VM	VM		VM FD VF
NE	VM	 # FM	VM		FM VM
NF	VM	 # VM	VM		VM FM
NG	VM	 # VM	VM		VM FD VF
NH	VM	 # VM	VM		VM VF FD
NI	VM	 # VM	VM		VM
NJ	VM	 # VM	VM		VM FM
NK	VM	 # VM	VM		VM VF FD
NL	VM	 # VM	VM		VM FD VF
NM	VM	 # VM	VM		VM FD VF
NN	VM	 # VM	VM		VM FM
NO	VM	 # VM	VM		VM FM
NP	VM	 # VM	VM		VM FD VF
NQ	VM	 # VM	VM		VM
NR	VM	 # VM	VM		VM FD VF
NS	VM	 # VM	VM		VM VF
NT	VM	 # VM	VM		VM FM
NU	VM	 # VM	VM		VM F
NV	VM	 # VF	VM		VF VM FD
NW	VM	 # VM	VM		VM FD VF
NX	VM	 # VM	VM		VM
NY	VM	 # VM	VM		VM F
NZ	VM	 # VM	VM		VM FM
OA	FM	 # FM			FM
OB	VF	 # VF*			
OC	FM	 # FM			FM
OD	FD	 # FD			FD
OE	FM	 # FM			FM
OF	FM	 # FM			FM
OG	FD	 # FD			FD
OH	FD	 # FD			FD
OI	VM	 # VM*			
OJ	FM	 # FM			FM
OK	FD	 # FD			FD
OL	FD	 # FD			FD
OM	FD	 # FD			FD
ON	FM	 # FM			FM
OO	FM	 # FM			FM
OP	FD	 # FD			FD
OQ	F	 # F*			
OR	FD	 # FD			FD
OS	FD	 # FD*			
OT	FM	 # FM			FM
OU	F	 # F			F
OV	FD	 # FD			FD
OW	FD	 # FD			FD
OX	VM	 # VM*			
OY	F	 # F			F
OZ	FM	 # FM			FM
PA	VD	 # FM	VD		FM VD
PB	VD	 # VD	VD		VD
PC	VD	 # FM	VD		FM VD
PD	VD	 # VF	VD		VF FD VD
PE	VD	 # FM	VD		FM VD
PF	VD	 # VD	VD		VD FM
PG	VD	 # VF	VD		VF FD VD
PH	VD	 # VD	VD		VD FD
PI	VD	 # VD	VD		VD
PJ	VD	 # FM	VD		FM VD VF
PK	VD	 # FD	VD		FD VD
PL	VD	 # VF	VD		VF FD VD
PM	VD	 # VF	VD		VF FD VD
PN	VD	 # FM	VD		FM VD
PO	VD	 # FM	VD		FM VD
PP	VD	 # VF	VD		VF VD FD
PQ	VD	 # VD	VD		VD
PR	VD	 # VF	VD		VF FD VD
PS	VD	 # VD	VD		VD
PT	VD	 # FM	VD		FM VD VF
PU	VD	 # VD	VD		VD F
PV	VD	 # FD	VD		FD VD
PW	VD	 # VF	VD		VF FD VD
PX	VD	 # VD	VD		VD
PY	VD	 # F	VD		F VD
PZ	VD	 # FM	VD		FM VD
QA	V	 # FM	V		FM V
QB	V	 # V	V		V
QC	V	 # FM	V		FM V
QD	V	 # FD	V		FD V
QE	V	 # FM	V		FM V
QF	V	 # FM	V		FM V
QG	V	 # FD	V		FD V
QH	V	 # FD	V		FD V
QI	V	 # V	V		V
QJ	V	 # FM	V		FM V
QK	V	 # FD	V		FD V
QL	V	 # FD	V		FD V
QM	V	 # FD	V		FD V
QN	V	 # FM	V		FM V
QO	V	 # FM	V		FM V
QP	V	 # FD	V		FD V
QQ	V	 # V	V		V
QR	V	 # FD	V		FD V
QS	V	 # V	V		V
QT	V	 # FM	V		FM V
QU	V	 # F	V		F V
QV	V	 # FD	V		FD V
QW	V	 # FD	V		FD V
QX	V	 # V	V		V
QY	V	 # F	V		F V
QZ	V	 # FM	V		FM V
RA	VD	 # FM	VD		FM VD VF
RB	VD	 # VD	VD		VD
RC	VD	 # VD	VD		VD FM
RD	VD	 # VD	VD		VD FD VF
RE	VD	 # FM	VD		FM VD VF
RF	VD	 # VD	VD		VD FM
RG	VD	 # VD	VD		VD VF FD
RH	VD	 # VD	VD		VD FD
RI	VD	 # VD	VD		VD
RJ	VD	 # VF	VD		VF VD FM
RK	VD	 # VD	VD		VD FD
RL	VD	 # VD	VD		VD FD VF
RM	VD	 # VD	VD		VD FD
RN	VD	 # VD	VD		VD FM VF
RO	VD	 # VD	VD		VD FM VF
RP	VD	 # VD	VD		VD VF FD
RQ	VD	 # VD	VD		VD
RR	VD	 # VD	VD		VD FD VF
RS	VD	 # VD	VD		VD
RT	VD	 # VD	VD		VD FM VF
RU	VD	 # VD	VD		VD F
RV	VD	 # VD	VD		VD FD
RW	VD	 # VD	VD		VD FD
RX	VD	 # VD	VD		VD
RY	VD	 # VD	VD		VD F
RZ	VD	 # VD	VD		VD FM
SA	VD	 # FM	VD		FM VF VD
SB	VD	 # VD	VD		VD
SC	VD	 # VD	VD		VD FM
SD	VD	 # VD	VD		VD FD VF
SE	VD	 # FM	VD		FM VD VF
SF	VD	 # VD	VD		VD FM
SG	VD	 # VD	VD		VD VF FD
SH	VD	 # VD	VD		VD FD
SI	VD	 # VD	VD		VD
SJ	VD	 # VD	VD		VD VF FM
SK	VD	 # VD	VD		VD FD
SL	VD	 # VD	VD		VD FD VF
SM	VD	 # FD	VD		FD VD VF
SN	VD	 # VD	VD		VD FM VF
SO	VD	 # FM	VD		FM VD VF
SP	VD	 # VF	VD		VF VD FD
SQ	VD	 # VD	VD		VD
SR	VD	 # VD	VD		VD VF FD
SS	VD	 # VD	VD		VD
ST	VD	 # FM	VD		FM VD VF
SU	VD	 # VD	VD		VD F
SV	VD	 # VD	VD		VD FD
SW	VD	 # FD	VD		FD VD VF
SX	VD	 # VD	VD		VD
SY	VD	 # VD	VD		VD F
SZ	VD	 # VD	VD		VD FM
TA	VD	 # FM	VD		FM VD VM
TB	VD	 # VD	VD		VD VM
TC	VD	 # FM	VD		FM VD VM
TD	VD	 # VF	VD		VF FD VD VM
TE	VD	 # FM	VD		FM VD VM
TF	VD	 # VD	VD		VD FM VM
TG	VD	 # VF	VD		VF FD VD VM
TH	VD	 # FD	VD		FD VD VM
TI	VD	 # VD	VD		VD VM
TJ	VD	 # FM	VD		FM VF VD VM
TK	VD	 # FD	VD		FD VD VM VF
TL	VD	 # VF	VD		VF FD VD VM
TM	VD	 # VF	VD		VF FD VD VM
TN	VD	 # FM	VD		FM VD VM
TO	VD	 # FM	VD		FM VD VM
TP	VD	 # VF	VD		VF FD VD VM
TQ	VD	 # VD	VD		VD VM
TR	VD	 # VF	VD		VF FD VD VM
TS	VD	 # VD	VD		VD VM
TT	VD	 # FM	VD		FM VD VM
TU	VD	 # VD	VD		VD VM F
TV	VD	 # FD	VD		FD VF VD VM
TW	VD	 # VF	VD		VF FD VD VM
TX	VD	 # VD	VD		VD VM
TY	VD	 # F	VD		F VD VM
TZ	VD	 # FM	VD		FM VD VM
UA	V	 # FM	V		FM V
UB	V	 # V	V		V
UC	V	 # FM	V		FM V
UD	V	 # FD	V		FD V
UE	V	 # FM	V		FM V
UF	V	 # FM	V		FM V
UG	V	 # FD	V		FD V
UH	V	 # FD	V		FD V
UI	V	 # V	V		V
UJ	V	 # FM	V		FM V
UK	V	 # FD	V		FD V
UL	V	 # FD	V		FD V
UM	V	 # FD	V		FD V
UN	V	 # FM	V		FM V
UO	V	 # FM	V		FM V
UP	V	 # FD	V		FD V
UQ	V	 # V	V		V
UR	V	 # FD	V		FD V
US	V	 # V	V		V
UT	V	 # FM	V		FM V
UU	V	 # F	V		F V
UV	V	 # FD	V		FD V
UW	V	 # FD	V		FD V
UY	V	 # F	V		F V
UZ	V	 # FM	V		FM V
VA	FM	 # FM			FM
VB	VF	 # VF*			
VC	FM	 # FM			FM
VD	FD	 # FD			FD
VE	FM	 # FM			FM
VF	FM	 # FM			FM
VG	FD	 # FD			FD
VH	FD	 # FD			FD
VI	VM	 # VM*			
VJ	FM	 # FM			FM
VK	FD	 # FD			FD
VL	FD	 # FD			FD
VM	FD	 # FD			FD
VN	FM	 # FM			FM
VO	FM	 # FM			FM
VP	FD	 # FD			FD
VQ	F	 # F*			
VR	FD	 # FD			FD
VS	FD	 # FD*			
VT	FM	 # FM			FM
VU	F	 # F			F
VV	FD	 # FD			FD
VW	FD	 # FD			FD
VX	VM	 # VM*			
VY	F	 # F			F
VZ	FM	 # FM			FM
WA	VM	 # FM	VM		FM VM VD
WB	VM	 # VM	VM		VM VD
WC	VM	 # FM	VM		FM VM VD
WD	VM	 # VF	VM		VF FD VM VD
WE	VM	 # FM	VM		FM VM VD
WF	VM	 # VM	VM		VM FM VD
WG	VM	 # VF	VM		VF FD VM VD
WH	VM	 # VM	VM		VM FD VD VF
WI	VM	 # VM	VM		VM VD
WJ	VM	 # FM	VM		FM VM VD VF
WK	VM	 # FD	VM		FD VF VM VD
WL	VM	 # VF	VM		VF FD VM VD
WM	VM	 # FD	VM		FD VF VM VD
WN	VM	 # FM	VM		FM VM VD
WO	VM	 # FM	VM		FM VM VD
WP	VM	 # VF	VM		VF FD VM VD
WQ	VM	 # VM	VM		VM VD
WR	VM	 # VF	VM		VF FD VM VD
WS	VM	 # VM	VM		VM VD
WT	VM	 # FM	VM		FM VM VD
WU	VM	 # VM	VM		VM VD F
WV	VM	 # FD	VM		FD VF VM VD
WW	VM	 # VF	VM		VF FD VM VD
WX	VM	 # VM	VM		VM VD
WY	VM	 # F	VM		F VM VD
WZ	VM	 # FM	VM		FM VM VD
XA	V	 # FM	V		FM V
XB	V	 # V	V		V
XC	V	 # FM	V		FM V
XD	V	 # FD	V		FD V
XE	V	 # FM	V		FM V
XF	V	 # FM	V		FM V
XG	V	 # FD	V		FD V
XH	V	 # FD	V		FD V
XI	V	 # V	V		V
XJ	V	 # FM	V		FM V
XK	V	 # FD	V		FD V
XL	V	 # FD	V		FD V
XM	V	 # FD	V		FD V
XN	V	 # FM	V		FM V
XO	V	 # FM	V		FM V
XP	V	 # FD	V		FD V
XQ	V	 # V	V		V
XR	V	 # FD	V		FD V
XS	V	 # V	V		V
XT	V	 # FM	V		FM V
XU	V	 # F	V		F V
XV	V	 # FD	V		FD V
XW	V	 # FD	V		FD V
XX	V	 # V	V		V
XY	V	 # F	V		F V
XZ	V	 # FM	V		FM V
YA	FM	 # FM			FM
YB	VF	 # VF			VF
YC	FM	 # FM			FM
YD	FD	 # FD			FD
YE	FM	 # FM			FM
YF	FM	 # FM			FM
YG	FD	 # FD			FD
YH	FD	 # FD			FD
YI	VM	 # VM*			
YJ	FM	 # FM			FM
YK	FD	 # FD			FD
YL	FD	 # FD			FD
YM	FD	 # FD			FD
YN	FM	 # FM			FM
YO	FM	 # FM			FM
YP	FD	 # FD			FD
YQ	F	 # F*			
YR	FD	 # FD			FD
YS	FD	 # FD*			
YT	FM	 # FM			FM
YU	F	 # F			F
YV	FD	 # FD			FD
YW	FD	 # FD			FD
YX	VM	 # VM*			
YY	F	 # F			F
YZ	FM	 # FM			FM
ZA	V	 # FM	V		FM V
ZB	V	 # V	V		V
ZC	V	 # FM	V		FM V
ZD	V	 # FD	V		FD V
ZE	V	 # FM	V		FM V
ZF	V	 # FM	V		FM V
ZG	V	 # FD	V		FD V
ZH	V	 # FD	V		FD V
ZI	V	 # V	V		V
ZJ	V	 # FM	V		FM V
ZK	V	 # FD	V		FD V
ZL	V	 # FD	V		FD V
ZM	V	 # FD	V		FD V
ZN	V	 # FM	V		FM V
ZO	V	 # FM	V		FM V
ZP	V	 # FD	V		FD V
ZQ	V	 # V	V		V
ZR	V	 # FD	V		FD V
ZS	V	 # V	V		V
ZT	V	 # FM	V		FM V
ZU	V	 # F	V		F V
ZV	V	 # FD	V		FD V
ZW	V	 # FD	V		FD V
ZX	V	 # V	V		V
ZY	V	 # F	V		F V
ZZ	V	 # FM	V		FM V

Miss: 2.49952043273157 %
