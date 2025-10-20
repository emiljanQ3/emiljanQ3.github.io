---
title: "Eksempler på kodeblokker"
date: 2025-10-20
categories: [docs, examples]
tags: [kotlin, nim, xml, markdown]
description: "Demonstrasjon av hvordan man legger inn kodeblokker i ulike språk samt et bilde i Markdown."
layout: post
---

# Eksempler på kodeblokker

Denne posten viser hvordan du kan legge inn kode i ulike språk (Kotlin, Nim og XML) i Markdown for Jekyll.

## Kotlin
```kotlin
data class Person(val id: Int, val navn: String)

fun hentAktivePersoner(alle: List<Person>): List<Person> =
		alle.filter { it.navn.isNotBlank() }

fun main() {
		val personer = listOf(Person(1, "Emil"), Person(2, ""))
		val aktive = hentAktivePersoner(personer)
		println("Aktive: ${aktive.map { it.navn }}")
}
```

Forklaring:
- Vi lager en enkel `data class`.
- Filtrerer bort tomme navn.
- Skriver ut resultatet.

## Nim
```nim
type Person = object
	id: int
	navn: string

proc hentAktivePersoner(alle: seq[Person]): seq[Person] =
	for p in alle:
		if p.navn.len > 0:
			result.add p

let personer = [Person(id: 1, navn: "Emil"), Person(id: 2, navn: "")]
let aktive = hentAktivePersoner(personer)
echo "Aktive: ", aktive.mapIt(it.navn)
```

Forklaring:
- Definerer en `object` type i Nim.
- Funksjon (proc) som filtrerer sekvensen.
- Bruker `mapIt` for å hente navn.

## XML
```xml
<?xml version="1.0" encoding="UTF-8"?>
<personer>
	<person id="1">
		<navn>Emil</navn>
	</person>
	<person id="2">
		<navn />
	</person>
</personer>
```

Forklaring:
- En rot-tag `<personer>` som inneholder flere `<person>` elementer.
- Tom `<navn />` viser fravær av verdi.

## Tips for Markdown kodeblokker
1. Bruk riktig språk etter de tre backticks for syntaks-highlighting (` ```kotlin `, ` ```nim `, ` ```xml `).
2. Unngå blanding av typografiske og vanlige backticks (i originalen var det feil med ´ vs `).
3. Hold kode kompakt og fokuser på ett poeng.
4. Legg til forklaring under hver blokk for kontekst.

## Bilde
Under er et lokalt generert eksempelbilde (SVG) for å demonstrere bruk av egne illustrasjoner:

![Lokalt generert abstrakt eksempelbilde](/assets/img/generated-cool-example.svg)

_Dette SVG-bildet er generert lokalt og kan fritt tilpasses. Fordel: ingen eksterne avhengigheter, rask lasting, og kan styles videre med CSS._

---
Har du flere språk du ønsker demonstrert? Legg igjen en kommentar eller opprett en issue.
