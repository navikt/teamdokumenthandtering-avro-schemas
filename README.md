# Doknotifikasjon

Avro-skjemaer for doknotifikasjon, for mer info for felter se på [confluence](https://confluence.adeo.no/display/BOA/doknotifikasjon+-+Funksjonell+Beskrivelse)

Avro skjema blir brukt på denne [applikasjonen](https://github.com/navikt/doknotifikasjon).

# Joarkhendelser

Avro-skjemaer for joarkhendelser, for mer info for felter se på [confluence](https://confluence.adeo.no/display/BOA/Joarkhendelser)

Avro skjema blir brukt på denne [applikasjonen](https://github.com/navikt/joarkhendelser).

# Sett opp applikasjonen

For å bruke denne maven artifact er det anbefalt at applikasjonen blir satt opp via github action. Github action setter authentication og annet via workflow filen. For å authenticate mot github package registry, må det opprettes en [Personal Github access token](https://github.com/settings/tokens) med `SSO enabled` og `read:packages=true`. Dette tokenet må bli lagt til som secret til repo, som skal brukes i workflow filen som `ACCES_TOKEN`. Dette tokent kan bli brukt slukt i workflow filen:

```
      env:
        GITHUB_USERNAME: x-access-token
        GITHUB_TOKEN: ${{ secrets.ACCESS_TOKEN }}
      run: "mvn install -X --settings .m2/maven-settings.xml --file pom.xml"
```

root pom skal se slikt ut:
```
	<repositories>
		<repository>
			<id>github</id>
			<url>https://maven.pkg.github.com/navikt/teamdokumenthandtering-avro-schemas</url>
		</repository>
	</repositories>

		<dependency>
			<groupId>no.nav.teamdokumenthandtering</groupId>
			<artifactId>teamdokumenthandtering-avro-schemas</artifactId>
			<version>...</version>
		</dependency>
```

settings.xml skal se slikt ut: ./m2/settings.xml

Følg denne guiden for å sette opp [maven package](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-apache-maven-registry)

# Local settup

For å få tilgang til Github Package Registry lokalt må mann sette opp ~./m2/settings.xml lokalt på laptop. Se på filen m2/maven-setting.xml filen for det som må settes opp.

Neste steg er å sette opp `GITHUB_USERNAME` and `GITHUB_TOKEN` i enviroment variables, disse verdiene må bli genrert via [Personal Github access token](https://github.com/settings/tokens) med `SSO enabled` og `read:packages=true`.
