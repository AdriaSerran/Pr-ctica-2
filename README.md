# Practica-2 - INTERRUPCIONES

Aquesta pràctica veurem com podem controlar l'estat de 2 leds amb l'ajuda d'un botó.

# Material

- ESP32
- 2 diodes LED
- Botó

# Funcionament

Primer definirem la tupla Button i un void que deinirà si el nostre botó ha estat apretat.

```c++
struct Button {
  const uint8_t PIN;
  uint32_t numberKeyPresses;
  bool pressed;
};

Button button1 = {18, 0, false};

void IRAM_ATTR isr() {
  button1.numberKeyPresses += 1;
  button1.pressed = true;
}
```

A continuació tenim el setup i el loop en veurem que s'aplica un if el qual ens dirà el nombre de vegades que el botó ha estat apretat i, que per tant, les vegades
que el led ha canviat d'estat.

```c++
void setup() {
  Serial.begin(115200);
  pinMode(button1.PIN, INPUT_PULLUP);
  attachInterrupt(button1.PIN, isr, FALLING);
}

void loop() {
  if (button1.pressed) {
    Serial.printf("Button 1 has been pressed %u times\n", button1.numberKeyPresses);
    button1.pressed = false;
}


static uint32_t lastMillis = 0;
if (millis() - lastMillis > 60000) {
  lastMillis = millis();
  detachInterrupt(button1.PIN);
  Serial.println("Interrupt Detached!");
}

}
```

Com ja he comentat a la primera pràctica, el primer dia no vam poder realitzar les pràctiques programades ja que ens faltava material. Hem realitzat el codi a casa
però malauradament seguíem sense disposar de tot el material (leds i botó), fet pel qual no puc adjuntar cap vídeo del funcionament.
