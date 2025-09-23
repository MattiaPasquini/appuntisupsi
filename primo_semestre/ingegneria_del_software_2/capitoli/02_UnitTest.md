# Ingegneria del Software 2

**Data lezione**: 16.09.2025 - 23.09.2025

**Autori**: Pasqui

**Disclaimer**: non è ancora finito

[back](./../index.md)

# Capitolo 2 - Unit Test

## Importanza dei test
Molti sviluppatori sanno che dovrebbero scrivere i test ma spesso non lo fanno per fretta o eccessiva fiducia in sé stessi. Di conseguenza il codice è meno stabile, ci sarà molto più perdita di tempo per sistemare ecc.
Per questo il test deve dovrebbe integrare la programmazione.

Ci sono molti benifici, tra cui:
- **Cicli di sviluppo più brevi**: “code a little, test a little”.
- **Maggiore sicurezza e produttività**: sapere che il codice è corretto aumenta la fiducia.
- **Facilità di cambiamento**: con test automatici è più semplice fare refactoring.
- **Evoluzione naturale** verso concetti come [TDD (Test-Driven Development)](https://en.wikipedia.org/wiki/Test-driven_development).

## Test automatico vs manuale
I test manuali sono costosi e poco ripetibili, all'inizio sembra veloce ma con tempo diventa sempre più dispendioso ed è facile dimenticare di testare qualche funzionalità.
Mentre i test automatici richiedono un costo iniziale (scrittura del test) ma permettono di fare tante esecuzioni rapide e affidabili.

## XP (eXtreme Programming) e TDD
Prima scrivo il test e poi il codice per soddisfarlo. Questo porta a uno sviluppo incrementale, con strumenti che facilitano la generazione di codice mancante ("stubs") e il refactoring automatico.

## Come scrivere i test
- Dai dettagli ai test più generali (dal basso verso l'alto)
- Accumulare i vecchi test e richiamarli regolarmente
- Il controllo della correttezza deve essere automatico (evitare, se possibile, di analizzare manualmente l'output lungo)
- Facile da scrivere, senza "danneggiare" troppo il codice del programma da sviluppare
- La realizzazione del test può anticipare il codice da sviluppare (XP)

## JUnit
JUnit è un tool strafigo per testare le funzionalità del software.

Supponiamo di avere la classe `Calculator`.

```java
public class Calculator{
	public int multByTwo(int value) {
		return value * 2;
	}
}
```

Ora è possibile testare la funzione `multByTwo(int value)`:

```java
//...
import static org.junit.jupiter.api.Assertions.*;

public class CalulatorTest {
	//...
	
	@Test
	public void testMultByTwo () {
		Calculator calculator = new Calculator();
		assertEquals(0, calculator.multByTwo(0));
		assertEquals(10, calculator.multByTwo(5));
		assertEquals(-10, calculator.multByTwo(-5));
	}
}
```

Attenzione! I metodi assert sono importati staticamente!
Comunque ci sono tanti metodi tipo `assertEquals`, `assertFalse`, ecc. Ecco la [documentazione](https://docs.junit.org/current/user-guide/#writing-tests-assertions) oppure guarda l'immagine seguente:

![](./attachments/Pasted%20image%2020250924000754.png)


Altro esempio: `Money` e `MoneyBag`

```java
public class Money {
	private int amount;
	private String currency;
	public Money(int amount, String currency) {
		this.amount = amount;
		this.currency = currency;
	}
	public int getAmount() {
		return amount;
	}
	public String getCurrency() {
		return currency;
	}
}
```

Vorremmo specificare *equals* per distinguere tra gli oggetti con lo stesso valore.

```java
public boolean equals(Object other) {
	return ((other instanceof Money) &&
		((Money)other).getCurrency().equals(currency) &&
		((Money)other).getAmount() == amount);
}
```

Ora è il momento di scrivere il primo test:

```java
@Test
public void testEquals() {
	Money m12CHF = new Money(12, "CHF"); // (*)
	Money m14CHF = new Money(14, "CHF");
	assertEquals(m12CHF, new Money(12, "CHF"));
	assertFalse(m12CHF.equals(m14CHF));
}
```

Ora aggiungiamo un ulteriore funzionalità

```java
public Money add(Money money) {
	return new Money(amount + money.getAmount(), currency);
}
```

E scriviamo il test:

```java
public class MoneyTest {
	...
	@Test
	public void testSimpleAdd() {
		Money m12CHF= new Money(12,"CHF");
		Money m14CHF= new Money(14,"CHF");
		Money expected= new Money(26,"CHF");
		assertEquals(expected, m12CHF.add(m14CHF));
	}
}
```

Notiamo che nei metodi `testSimpleAdd` e `testEquals` hanno una cosa in comune che è inizializzazione delle due istanze `Money.`

```java
Money m12CHF= new Money(12,"CHF");
Money m14CHF= new Money(14,"CHF");
```

JUnit fornice una funzionalità che permette di fare un meccanismo di setup per istanziare gli oggetti una volta per tutti i test.

```java
public class MoneyTest {
	private Money f12CHF;
	private Money f14CHF;
	
	@BeforeEach
	protected void setUp() {
		f12CHF= new Money(12, "CHF");
		f14CHF= new Money(14, "CHF");
	}
	...
}
```

I metodi con l'annotazione `@BeforeEach` permette di chiamare quel metodo prima di ogni test, per garantire che non ci siano *side-effects*. Poi esistono anche `@AfterEach`, `@BeforeAll`, `@AfterAll`. I nomi sono abbastanza *self-explanatory* per capire cosa fanno lol.

Ora introduciamo `MoneyBag` per gestire più valute contemporaneamente.

ANCORA DA FINIRE SORRY

