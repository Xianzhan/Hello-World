<!-- TOC -->

- [继承结构](#继承结构)

<!-- /TOC -->

# 继承结构

```shell
                             Object
        +----------------------+------------------+
        |                                         |
      Container                                AWT Components...
   +----+-----+-----------------+
   |          |                 |
 Panel      Window          JComponent
   |       +--+-------+      +--+------+--------------+--------+------------+
   |       |          |      |         |              |        |            |
 Applet  Frame     Dialog  JLable  JAastractButton JPanel JTextComponent JScrollPane
   |       |          |          +-----+------+            +---+----+
   |       |          |          |            |            |        |
 JApplet JFrame     JDialog     JButton  JToggleButton JTextField JTextArea
```