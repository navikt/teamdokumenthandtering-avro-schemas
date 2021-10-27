# Doknotifikasjon

Avro-skjemaer for doknotifikasjon, for mer info for felter se på [confluence](https://confluence.adeo.no/display/BOA/doknotifikasjon+-+Funksjonell+Beskrivelse)

Avro skjema blir brukt på denne [applikasjonen](https://github.com/navikt/doknotifikasjon).

# Joarkhendelser

Avro-skjemaer for joarkhendelser, for mer info for felter se på [confluence](https://confluence.adeo.no/display/BOA/Joarkhendelser)

Avro skjema blir brukt på denne [applikasjonen](https://github.com/navikt/joarkhendelser).

# Sett opp applikasjonen

For å bruke denne maven artifact er det anbefalt at applikasjonen blir satt opp via github action. Github action setter authentication og annet via workflow filen.

Start med å legge til dette i pom root filen:
```
<dependency>
    <groupId>no.nav.doknotifikasjon</groupId>
    <artifactId>teamdokumenthandtering-avro-schemas</artifactId>
    <version>25962ac5</version>
</dependency>

<repository>
    <id>github</id>
    <name>nav github</name>
    <url>https://github-package-registry-mirror.gc.nav.no/cached/maven-release</url>
</repository>
```

Legge til dette i workflow filen: 
```


```

# Local settup

For å få tilgang til Github Package Registry lokalt må mann sette opp ~./m2/settings.xml lokalt på laptop. Se på filen m2/maven-setting.xml filen for det som må settes opp.

Neste steg er å sette opp `GITHUB_USERNAME` and `GITHUB_TOKEN` i enviroment variables, disse verdiene må bli genrert via [Personal Github access token](https://github.com/settings/tokens) med `SSO enabled` og `read:packages=true`.
