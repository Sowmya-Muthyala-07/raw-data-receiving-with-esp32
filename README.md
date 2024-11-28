#include <IRremote.h>

#define IR_RECEIVE_PIN 32 // Pin where the IR receiver module is connected

void setup() {
  Serial.begin(115200); // Initialize serial communication
  IrReceiver.begin(IR_RECEIVE_PIN, ENABLE_LED_FEEDBACK); // Start IR receiver
  Serial.println("Ready to receive IR signals.");
}

void loop() {
  if (IrReceiver.decode()) { // Check if a signal is received
    Serial.println("IR Signal Received!");

    // Print protocol and command
    if (IrReceiver.decodedIRData.protocol != UNKNOWN) {
      Serial.print("Protocol: ");
      Serial.println(IrReceiver.decodedIRData.protocol);
      Serial.print("Address: 0x");
      Serial.println(IrReceiver.decodedIRData.address, HEX);
      Serial.print("Command: 0x");
      Serial.println(IrReceiver.decodedIRData.command, HEX);
    } else {
      Serial.println("Unknown protocol. Capturing raw data...");
    }

    // Print raw signal data
    Serial.print("Raw data (in microseconds): ");
    for (unsigned int i = 0; i < IrReceiver.decodedIRData.rawDataPtr->rawlen; i++) {
      Serial.print(IrReceiver.decodedIRData.rawDataPtr->rawbuf[i] * MICROS_PER_TICK);
      Serial.print(" ");
    }
    Serial.println();

    IrReceiver.resume(); // Prepare to receive the next signal
  }
}
