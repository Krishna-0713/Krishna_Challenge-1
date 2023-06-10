# Krishna_Challenge-1

#include <stdio.h>
#include <stdint.h>
#include <stdbool.h>

#define MAX_PACKET_DATA_LENGTH 50

typedef struct {
    uint8_t id;
    uint8_t data_length;
    uint8_t data[MAX_PACKET_DATA_LENGTH];
    uint16_t crc;
} data_packet_t;

uint16_t calculate_crc(const data_packet_t *packet) {
    uint16_t crc = 0;
    for (int i = 0; i < packet->data_length; i++) {
        crc ^= packet->data[i];
    }
    return crc;
}

bool is_data_packet_corrupted(const data_packet_t *packet) {
    uint16_t calculated_crc = calculate_crc(packet);
    return calculated_crc != packet->crc;
}

int main() {
    data_packet_t packet;
    packet.id = 1;
    packet.data_length = 10;
    for (int i = 0; i < packet.data_length; i++) {
        packet.data[i] = i;
    }
    packet.crc = calculate_crc(&packet);

    printf("Packet is %scorrupted.\n", is_data_packet_corrupted(&packet) ? "" : "not ");

    return 0;
}

// The code first calculates the CRC checksum for the data packet.
// It then compares the calculated CRC checksum to the CRC checksum that is stored in the packet.
// If the two checksums do not match, then the packet is corrupted. 
// This code can be used to detect data corruption in any type of data transmission, including wireless communication, wired communication, and storage. 
// This will print the following output: 
// Packet is not corrupted. 
