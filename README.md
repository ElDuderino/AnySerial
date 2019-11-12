AnySerial
=========

Arduino wrapper class for HardwareSerial, SoftwareSerial &amp; AltSoftSerial

Forked from AnySerial to add Leonardo support (for Serial_)

As others have commented:

The problem is: for some boards, Serial is not defined as HardwareSerial, but instead as Serial_.

HardwareSerial.h, line 97:
````
#if defined(UBRRH) || defined(UBRR0H)
  extern HardwareSerial Serial;
#elif defined(USBCON)
  #include "USBAPI.h"
//  extern HardwareSerial Serial_;  
#endif
````
USBAPI.h, line 25:
````
//================================================================================
//================================================================================
//  Serial over CDC (Serial1 is the physical port)

class Serial_ : public Stream
{
private:
    ring_buffer *_cdc_rx_buffer;
public:
    void begin(uint16_t baud_count);
    void end(void);

    virtual int available(void);
    virtual void accept(void);
    virtual int peek(void);
    virtual int read(void);
    virtual void flush(void);
    virtual size_t write(uint8_t);
    using Print::write; // pull in write(str) and write(buf, size) from Print
    operator bool();
};
extern Serial_ Serial;
````
