# En kort guide i att utvärdera Öppen programvara

_av [Open Source Security Foundation (OpenSSF)](https://openssf.org) [Best Practices Working Group](https://best.openssf.org/), 2023-11-21_

Innan du som utvecklare använder öppen programvaru-beroenden eller verktyg; identifiera och utvärdera de ledande kandidaterna mot dina behov.
För att utvärdera en möjlig kandidats säkerhet och hållbarhet, fråga dig  (alla verktyg eller tjänster som visas är exempel):

1. **Kan du undvika att lägga till beroendet?** Kan du använda ett befintligt (möjligtvis indirekt) beroende istället? Varje nytt beroende ökar attackytan (en förvanskning av det nya beroendet, eller dess transitiva beroenden, kan förvanska systemet).
2. **Utvärderar du rätt version?** Se till att du utvärderar den avsedda versionen av programvaran, inte en fork eller en angriparkontrollerad fork. Följande tekniker hjälper till att motverka den vanliga "typosquatting"-attacken (där en angripare skapar ett "nästan korrekt" namn).
   1. Kontrollera dess namn och projektets webbplats.
   2. Verifiera fork-förhållandet på GitHub/GitLab.
   3. Kontrollera om projektet är knutet till en stiftelse (i så fall bör du kunna komma åt den officiella källkoden från stiftelsens webbplats).
   4. Kontrollera dess skapelsestid och dess popularitet.
3. **Förvaltas det?** Övergivna programvaror är en risk; de flesta programvaror behöver löpande underhåll. Är det övergivet är det troligen osäkert.
   1. Har det skett någon betydande aktivitet (t.ex. commits) det senaste året?
   2. När skedde dess senaste utgåva (var det mindre än ett år sedan)?
   3. Finns det flera förvaltare, helst från olika organisationer?
   4. Finns det senare utgåvor eller tillkännagivanden från förvaltarna?
   5. Indikerar dess versionssträng instabilitet (t.ex. börjar med "0", inkluderar "alpha" eller "beta", etc.)?
4. **Finns det bevis på att dess utvecklare arbetar för att göra det säkert?**
   1. Avgör om projektet har förtjänat (eller är på god väg till att förtjäna) ett [Open Source Security Foundation (OpenSSF) Best Practices-märke](https://www.bestpractices.dev/).
   2. Se över informationen på [https://deps.dev](https://deps.dev/), inklusive dess [OpenSSF Scorecards](https://github.com/ossf/scorecard)-poäng och kända sårbarheter.
   3. Avgör om paketets beroenden är (relativt) uppdaterade.
   4. Avgör om det finns dokumentation som förklarar varför det är säkert ("assurans").
   5. Finns det automatiserade tester i dess CI-pipeline? Vad är dess testtäckning?
   6. Rättar projektet programfel (särskilt säkerhetsrelaterade) på ett perodiskt sätt? Släpper de säkerhetsrättningar för äldre utgåvor? Har de en LTS (Long Time Support)-version?
   7. Använder utvecklarna kodsamverkansplattformens säkerhetsfunktioner där det är tillämpligt (t.ex. om de är på GitHub eller GitLab, använder de branch-skydd)?
   8. Identifiera säkerhetsrevision och om problem som hittats har rättats. Säkerhetsrevisioner är relativt ovanliga, men se OpenSSF:s "[Säkerhetsgranskningar](https://github.com/ossf/security-reviews)".
   9. Använd [SAFECode:s guide _Principles for Software Assurance Assessment_](https://safecode.org/resource-managing-software-security/principles-of-software-assurance-assessment/) (2019), en flernivåansats för att undersöka programvarans säkerhet.
   10. Är aktuell versionen fri från kända viktiga sårbarheter (särskilt länge kända)? Organisationer kan vilja implementera [OpenChain](https://www.openchainproject.org/) [Security Assurance Specification 1.1](https://github.com/OpenChain-Project/Security-Assurance-Specification/tree/main/Security-Assurance-Specification/1.1/en) för att systematiskt kontrollera kända sårbarheter vid införande, och, när nya sårbarheter avslöjas offentligt.
   11. Tillämpar de flera av praxis från [Concise Guide for Developing More Secure Software](https://best.openssf.org/Concise-Guide-for-Developing-More-Secure-Software)?
5. **Är det lätt att använda på ett säkert sätt?**
   1. Är standardkonfigurationen och "enkla exempel" säkra (t.ex. kryptering påslagen som standard i nätverksprotokoll)? Om inte, undvik det.
   2. Är dess gränssnitt/API designat för att vara lätt att använda på ett säkert sätt (t.ex. om gränssnittet implementerar ett språk, stöder det parameteriserade frågor)?
   3. Finns det vägledning om hur man använder det säkert?
6. **Finns det instruktioner för hur man rapporterar sårbarheter?** Se [Guide för att implementera en process för sårbarhetsavslöjande i öppen källkodprojekt](https://github.com/ossf/oss-vulnerability-guide/blob/main/maintainer-guide.md#guide-to-implementing-a-coordinated-vulnerability-disclosure-process-for-open-source-projects) för vägledning.
7. **Har det någon betydande användning?** Programvara med många användare eller stora användarbaser kan vara olämpligt för din användning. Dock är det mer sannolikt att allmänt använd programvara har information om hur man använder det säkert, och att flera kommer att bry sig om dess säkerhet. Kontrollera om ett liknande namn är mer populärt - det kan indikera en typosquatting-attack.
8. **Vad är programvarans licens?** Licenser är tekniskt sett inte säkerhet, men licenser kan ha en betydande inverkan på säkerhet och hållbarhet. Se till att varje komponent har en licens, att det är en allmänt använd [OSI-licens](https://opensource.org/licenses) om det är öppen källkod, och att det är förenligt med din avsedda användning. Projekt som inte tillhandahåller tydlig licensinformation är mindre benägna att följa andra goda praxis, som leder till säker programvara.
9. **Vad händer vid ett testtillägg?** Försök att lägga till beroendet som ett test, helst i en isolerad miljö, för att undersöka dess påverkan:
   1. Visar det skadligt beteende, t.ex. försöker det hämta in känslig data?
   2. Lägger det till oväntade eller onödiga indirekta beroenden i produktionen? Till exempel, inkluderar det produktionsberoenden som bara krävs vid utvecklingstid eller testtid istället? Om så är fallet, skulle deras förvaltare vara villiga att fixa det? Varje nytt beroende är ett potentiellt supportproblem eller leveranskedjeattack, så det är klokt att minimera dem.
10. **Vad är resultaten av en kodutvärdering?** Även en kort granskning av programvaran (av dig, någon du anställer eller någon annan), tillsammans med senaste ändringarna, kan ge dig viss insikt. Några saker att tänka på:
    1. När du granskar dess källkod, finns det i koden indikationer på att utvecklarna försökte utveckla säker programvara (som rigorös indata validering av opålitliga indata samt användning av parameteriserade argument)?
    2. Finns det bevis på osäker/ ofullständig programvara (t.ex. många TODO-uttalanden)?
    3. Vilka är "topp"-problemen som rapporteras av statiska analystjänster?
    4. Finns det bevis på att programvaran är skadlig? Enligt [_Backstabber’s Knife Collection_](https://arxiv.org/abs/2005.09535), kontrollera installationsskript/rutiner för skadlighet, kontrollera dataexfiltrering från **~/.ssh** och miljövariabler, och leta efter kodade/ fördolda värden som exekveras. Undersök de senaste commitarna för misstänkt kod (en angripare kan ha lagt till dem nyligen).
    5. Överväg att köra programvaran i en sandbox för att försöka utlösa och upptäcka skadlig kod.
    6. Överväg att köra alla definierade testfall för att säkerställa att programvaran klarar dem.
    7. Se [OpenSSF:s lista över säkerhetsgranskningar](https://github.com/ossf/security-reviews/blob/main/Overview.md#readme).

Andra resurser att överväga:

1. [Tidelift's guide för att välja paket (February 2021)](https://tidelift.com/subscription/choosing-open-source-packages-well), Tidelift
2. [Hur man utvärderar Öppen Programvara / Fri Programvara](https://dwheeler.com/oss_fs_eval.html)

Vi välkomnar förslag och uppdateringar! Vänligen öppna ett [ärende](https://github.com/ossf/wg-best-practices-os-developers/issues/) eller skicka en [pull request](https://github.com/ossf/wg-best-practices-os-developers/pulls).
