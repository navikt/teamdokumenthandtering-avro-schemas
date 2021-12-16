# Fellesrepo for Avro-skjema i Team Dokumentløsninger
Følgende apper i Team Dokumentløsninger produserer meldinger av typen Avro, og vi har kategorisert de etter bruksområde.

Avro-skjema som også blir brukt av andre team:
- [joarkhendelser](https://github.com/navikt/joarkhendelser) - beskrivelse av feltene i Avro-skjemaet finner du [her](https://confluence.adeo.no/display/BOA/Joarkhendelser)

Avro-skjema som kun blir brukt internt i teamet:
- [doknotifikasjon](https://github.com/navikt/doknotifikasjon) - beskrivelse av feltene i Avro-skjemaet finner du [her](https://confluence.adeo.no/display/BOA/doknotifikasjon+-+Funksjonell+Beskrivelse)
- [safselvbetjening](https://github.com/navikt/safselvbetjening) - WIP

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
og må inkludere dependency
```
    <dependency>
        <groupId>no.nav.teamdokumenthandtering</groupId>
        <artifactId>teamdokumenthandtering-avro-schemas</artifactId>
        <version>...</version>
    </dependency>
```

I tillegg bør settings.xml-filen inneholde det som ligger [her](https://github.com/navikt/teamdokumenthandtering-avro-schemas/blob/master/m2/maven-settings.xml).

Følg denne guiden for å sette opp [maven package](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-apache-maven-registry).

# Lokalt oppsett

For å få tilgang til Github Package Registry lokalt må man sette opp ~./m2/settings.xml lokalt på laptop. Se på filen m2/maven-setting.xml filen for det som må settes opp.

Neste steg er å sette opp `GITHUB_USERNAME` and `GITHUB_TOKEN` som miljøvariabler. Disse verdiene må bli generert via [Github Personal Access Token](https://github.com/settings/tokens) med `SSO enabled` og `read:packages=true`.

## Henvendelser
Spørsmål om koden eller prosjektet kan rettes til Team Dokumentløsninger på:
* [\#Team Dokumentløsninger](https://app.slack.com/client/T5LNAMWNA/C6W9E5GPJ)