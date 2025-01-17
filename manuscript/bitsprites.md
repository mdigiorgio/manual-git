<!-- Appendix sprites with bit twiddling TODO -->
# Appendix B
## Moving Sprites

Now for some real fun. Moving sprites in hardware, and not having to worry
about clipping, buffers, or anything, is such a wonderful feeling. To move a
sprite, we simply change some attributes in that sprite's `SpriteEntry`.
Attribute 1 in a sprite always contains, in bits 0-8, the X position of the
sprite. Attribute 0, among other things, contains the Y position of the sprite,
in bits 0-7. I have provided some bitmasks we can use in `sprites.h`. <!-- XXX
TODO see if these defines are contained someplace in libnds and see what the
sprite examples do -->

```C++
//Move a Sprite
void moveSprite(SpriteEntry * spriteEntry, u16 x, u16 y) {
    spriteEntry->attribute[1] &= 0xFE00;
    spriteEntry->attribute[1] |= (x & 0x01FF);
    spriteEntry->attribute[0] &= 0xFF00;
    spriteEntry->attribute[0] |= (y & 0x00FF);
}
```

### Setting Sprite Priority

To the priority of the sprite, we use attribute 2 of the sprite. Bits XXX
though XXX contain the priority. I have provided a bit mask in `sprites.h` that
we can use. libnds provides the means of setting the priority bits through the
`ATTR2_PRIORITY` macro.

```C++
void setSpritePriority(SpriteEntry * spriteEntry, int priority) {
  spriteEntry->attribute[2] &= ~ATTR2_PRIORITY_MASK;
  spriteEntry->attribute[2] |= ATTR2_PRIORITY(priority);
}
```
