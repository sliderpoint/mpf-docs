How to configure LEDs (P-ROC/P3-ROC)
====================================

+------------------------------------------------------------------------------+
| Related Config File Sections                                                 |
+==============================================================================+
| :doc:`/config/leds`                                                          |
+------------------------------------------------------------------------------+

This guide explains how to configure MPF to use LEDs attached to a Multimorphic
PD-LED board with either a P-ROC or P3-ROC.

Note that if you're using a P-ROC/P3-ROC and you want to use serial-controlled
LEDs (NeoPixels, etc.), then you can do that with a P-ROC/P3-ROC by using a
:doc:`FadeCandy </hardware/fadecandy/index>` instead of a PD-LED. You can also
mix-and-match PD-LEDs and FadeCandy LEDs.

Understanding the PD-LED board
------------------------------

The PD-LED controls up to 84 individual LED elements, which can be used to
control individual single color LEDs, or (more likely), combined into groups to
control RGB LEDs.

The PD-LED uses a "direct" connection method for LEDs, where each LED
has connections for each color element running back to the PD-LED. This is a
different architecture than the serial-controlled "Neo Pixel" type LEDs that
other hardware uses.

LED number:
-----------

Since the PD-LED board directly drives single color LED outputs, when you use
it with RGB LEDs, you combine three outputs into a single RGB LED. The PD-LED
supports both common cathode (common ground) and common anode (common 3.3v)
LEDs, so each LED you buy has four pins (red, green, blue, and
common). When you configure the hardware number for a PD-LED RGB LED, you
specify four parts, separated by dashes:

1. The address of the PD-LED board on the serial chain (as configured via the
   DIP switches on the PD-LED.
2. The output number of the red element.
3. The output number of the green element.
4. The output number of the blue element.

You separate those with dashes, so an example PD-LED configuration might look
like this:

.. code-block:: mpf-config

   lights:
      l_led0:
         number: 8-0-1-2

The example above configures "l_led0” as the LED connected to PD-LED board at
address 8, using outputs 0, 1, and 2 as its red, green, and blue connections.

polarity:
---------

The PD-LED allows you to use either common anode or common cathode LEDs. (See
the PD-LED documentation for details. The type of LED would dictate whether you
hook it up between the PD-LED’s output and ground, or between the output and
3.3v.) You can then use the config file to specify which type of LED you have,
such as:

.. code-block:: mpf-config

   lights:
      l_shoot_again:
         number: 8-60-61-62
         platform_settings:
            polarity: True

**True** = common cathode (or common ground),
**False** = common anode (or common 3.3V)

Note that DIP Switch 6 on the PD-LED board controls whether the “default” state
of the LEDs after a reset is high or low. Basically it’s whether all the LEDs
turn on or turn off when the board is reset. Which position does what is
dependent on whether you’re controlling the anode or the cathode with your
outputs, so basically if you turn on your PD-LED and all your LEDs turn on,
then flip DIP switch 6 on the PD-LED to the opposite position and power cycle
the board.
