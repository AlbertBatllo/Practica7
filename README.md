# Practica7: Busos de Comunicació III (I2S)

L'objectiu de la pràctica és descriure el funcionament del bus I2S("Inter-Intergrated Circuit Sound") i fer una pràctica per comprendre'n el funcionament

La tecnologia I2S permet transmetre senyals amb mostres de 4 a 32 bits, on l'audio es troba mostrejat a una freqüencia de mostreig concreta, i el valor de cada mostra es troba modulat en forma de codi.

Aquest bus és molt utilitzat en dispositius d'audio digital.

## Codi

```
#include "Arduino.h"
#include "AudioGeneratorAAC.h"
#include "AudioOutputI2S.h"
#include "AudioFileSourcePROGMEM.h"
#include "sampleaac.h"

AudioFileSourcePROGMEM *in;
AudioGeneratorAAC *aac;
AudioOutputI2S *out;

void setup(){

  Serial.begin(115200);
  in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));
  aac = new AudioGeneratorAAC();
  out = new AudioOutputI2S();
  out -> SetGain(0.125);
  out -> SetPinout(26,25,22);
  aac->begin(in, out);

}

void loop(){
  if (aac->isRunning()) {
    aac->loop();
    } else {

      aac -> stop();
      Serial.printf("Sound Generator\n");
      delay(1000);
  }
}
```


# Explicació del codi

**Llibraries:**

S'utlitzen diferents llibreries compreses dins la llibreria "ESP8266Audio". Pel bus I2S s'utilitza la llibreria "AudioOutputI2S.h", i el fitxer d'audio "sampleaac.h" serà el que reproduirà el codi enviat:

```
#include "AudioGeneratorAAC.h"
#include "AudioOutputI2S.h"
#include "AudioFileSourcePROGMEM.h"
#include "sampleaac.h"

```

**Set Up:**

```
void setup(){

  Serial.begin(115200);
  in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));
  aac = new AudioGeneratorAAC();
  out = new AudioOutputI2S();
  out -> SetGain(0.125);
  out -> SetPinout(26,25,22);
  aac->begin(in, out);

}
```

**Loop:**

```
void loop(){
  if (aac->isRunning()) {
    aac->loop();
    } else {

      aac -> stop();
      Serial.printf("Sound Generator\n");
      delay(1000);
  }
}
