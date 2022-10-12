# Fellesrepo for Avro-skjema i Team Dokumentløsninger
Nedenfor følger en liste over Avroskjema produsert av Team Dokumentløsninger, og hvilke topicer en kan konsumere meldingene på. 

For å kunne konsumere meldingene må du få tilgang av Team Dokumentløsninger i Git-repoet [dokumenthandtering-iac](https://github.com/navikt/dokumenthandtering-iac).
Topicer uten q1-suffiks gir tilganger for både q2 og prod. Som hovedregel er det topicene med aapen i navnet som blir brukt utenfor teamet.

Det er i hovedsak følgende applikasjoner som produserer Avro-meldinger:
### [joarkhendelser](https://github.com/navikt/joarkhendelser)
  - gir andre fagsystem beskjed når det skjer endringer (insert eller update) på en inngående journalpost, uavhengig av hvem som har gjort endringen. Ved å filtrere på tema kan ulike fagsystem enkelt få beskjed om oppdateringar som gjelder dem.
  - beskrivelse av feltene i Avro-skjemaet finner du i [joarkhendelser-doken](https://confluence.adeo.no/display/BOA/Joarkhendelser)
  - Avro-skjema relatert til joarkhendelser:
    - [joarkjournalfoeringhendelser](src/main/avro/joarkhendelser/joarkjournalfoeringhendelser.avsc) er tilgjengelig på topic [aapen-dok-journalfoering](https://github.com/navikt/dokumenthandtering-iac/blob/master/kafka-aiven/aapen-dok-journalfoering/topic.yaml)
    - [joarkjournalfoeringhendelser](src/main/avro/joarkhendelser/joarkjournalfoeringhendelser.avsc) er tilgjengelig på topic [aapen-dok-journalfoering-q1](https://github.com/navikt/dokumenthandtering-iac/blob/master/kafka-aiven/aapen-dok-journalfoering-q1/topic.yaml)
    
### [doknotifikasjon](https://github.com/navikt/doknotifikasjon-2)
  - sender notifikasjon (varsel) til mottaker via epost eller sms.
  - beskrivelse av feltene i Avro-skjemaet finner du i [doknotifikasjon-doken](https://confluence.adeo.no/display/BOA/doknotifikasjon+-+Funksjonell+Beskrivelse)
  - Avro-skjema relatert til doknotifikasjon:
    - [notifikasjon](src/main/avro/doknotifikasjon/notifikasjon.avsc) er tilgjengelig på topic [privat-dok-notifikasjon](https://github.com/navikt/dokumenthandtering-iac/blob/master/kafka-aiven/privat-dok-notifikasjon/topic.yaml)
    - [notifikasjonMedKontaktInfo](src/main/avro/doknotifikasjon/notifikasjonMedKontaktInfo.avsc) er tilgjengelig på topic [privat-dok-notifikasjon-med-kontakt-info](https://github.com/navikt/dokumenthandtering-iac/blob/master/kafka-aiven/privat-dok-notifikasjon-med-kontakt-info/topic.yaml)
    - [notifikasjonEpost](src/main/avro/doknotifikasjon/notifikasjonEpost.avsc) er tilgjengelig på topic [privat-dok-notifikasjon-epost](https://github.com/navikt/dokumenthandtering-iac/blob/master/kafka-aiven/privat-dok-notifikasjon-epost/topic.yaml)
    - [notifikasjonSms](src/main/avro/doknotifikasjon/notifikasjonSms.avsc) er tilgjengelig på topic [privat-dok-notifikasjon-sms](https://github.com/navikt/dokumenthandtering-iac/blob/master/kafka-aiven/privat-dok-notifikasjon-sms/topic.yaml)
    - [notifikasjonStopp](src/main/avro/doknotifikasjon/notifikasjonStopp.avsc) er tilgjengelig på topic [privat-dok-notifikasjon-stopp](https://github.com/navikt/dokumenthandtering-iac/blob/master/kafka-aiven/privat-dok-notifikasjon-stopp/topic.yaml)
    - [notifikasjonStatus](src/main/avro/doknotifikasjon/notifikasjonStatus.avsc) er tilgjengelig på topic [aapen-dok-notifikasjon-status](https://github.com/navikt/dokumenthandtering-iac/blob/master/kafka-aiven/aapen-dok-notifikasjon-status/topic.yaml)

### [safselvbetjening](https://github.com/navikt/safselvbetjening)
  - informerer dokdistdittnav om når et dokument blir lest av bruker
  - Avro-skjema relatert til safselvbetjening (kun brukt internt i teamet)
    - [hoveddokumentLest](src/main/avro/doknotifikasjon/hoveddokumentLest.avsc) er tilgjengelig på topic [privat-dokdistdittnav-lestavmottaker](https://github.com/navikt/dokumenthandtering-iac/blob/master/kafka-aiven/privat-dokdistdittnav-lestavmottaker/topic.yaml)
    - [hoveddokumentLest](src/main/avro/doknotifikasjon/hoveddokumentLest.avsc) er tilgjengelig på topic [privat-dokdistdittnav-lestavmottaker-q1](https://github.com/navikt/dokumenthandtering-iac/blob/master/kafka-aiven/privat-dokdistdittnav-lestavmottaker-q1/topic.yaml)

## Oppsett for å kunne bruke Avro-skjemaene i din applikasjon
For å hente Avro-skjemaet som et Maven-artifact er det anbefalt at applikasjonen bruker Github Actions, slik at authentication blir gjort i workflow-filen. 
For å authenticate mot Github Package Registry må det opprettes en [Github Personal Access Token](https://github.com/settings/tokens) med `SSO enabled` og `read:packages=true`. 
Dette tokenet må bli lagt til som secret til repo og brukes i workflow-filen som `ACCESS_TOKEN`. Dette tokenet kan bli brukt slik i workflow-filen:

```
      env:
        GITHUB_USERNAME: x-access-token
        GITHUB_TOKEN: ${{ secrets.ACCESS_TOKEN }}
      run: "mvn install -X --settings .m2/maven-settings.xml --file pom.xml"
```

Root-POM trenger også
```
	<repositories>
		<repository>
			<id>dok-github</id>
			<url>https://maven.pkg.github.com/navikt/teamdokumenthandtering-avro-schemas</url>
		</repository>
	</repositories>
```
og må inkludere [nyeste versjon av dependency](https://github.com/navikt/teamdokumenthandtering-avro-schemas/packages/1065405))
```
    <dependency>
        <groupId>no.nav.teamdokumenthandtering</groupId>
        <artifactId>teamdokumenthandtering-avro-schemas</artifactId>
        <version>VERSJONSNUMMER</version>
    </dependency>
```

I tillegg bør settings.xml-filen inneholde det som ligger i [filen maven-settings.xml](https://github.com/navikt/teamdokumenthandtering-avro-schemas/blob/master/m2/maven-settings.xml).

Følg denne guiden for å sette opp [maven package](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-apache-maven-registry).

## Lokalt oppsett

For å få tilgang til Github Package Registry lokalt må man sette opp ~./m2/settings.xml lokalt på laptop. Se på filen m2/maven-setting.xml filen for det som må settes opp.

Neste steg er å sette opp `GITHUB_USERNAME` and `GITHUB_TOKEN` som miljøvariabler. Disse verdiene må bli generert via [Github Personal Access Token](https://github.com/settings/tokens) med `SSO enabled` og `read:packages=true`.

### Henvendelser
Spørsmål om koden eller prosjektet kan rettes til [Slack-kanalen for \#Team Dokumentløsninger](https://nav-it.slack.com/archives/C6W9E5GPJ)
