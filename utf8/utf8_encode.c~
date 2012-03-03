#include <stdlib.h>
#include <stdio.h>

struct utf8_char_ {
  short size;
  char  *octets;
};

typedef struct utf8_char_ utf8_char;
typedef int unic_char; 

char utf8_1st_byte_mask(unic_char ch, short size)
{
  char octet = 0x0; // 0x80
  
  if(1 == size)
  {
    //octet = 0xEF & ch;
    octet = 0x00 | (ch >> 0); 
  }
  else if(2 == size)
  {
    octet = 0xC0 | (ch >> 6);
  }
  else if(3 == size)
  {
    octet = 0xE0 | (ch >> 12);
  }  
  else if(4 == size)
  {
    octet = 0xF0 | (ch >> 18);
  }
  
  return octet;
}

char utf8_2nd_byte_mask(unic_char ch, short size)
{
  char octet = 0x0; // 0x80
  
  if(2 == size)
  {
    octet = 0x80 | ((ch >> 0) & 0x3F);
  }
  else if(3 == size)
  {
    octet = 0x80 | ((ch >> 6) & 0x3F);
  }  
  else if(4 == size)
  {
    octet = 0x80 | ((ch >> 12) & 0x3F);
  }
  
  return octet;
}

char utf8_3rd_byte_mask(unic_char ch, short size)
{
  char octet = 0x0;
  
  if(3 == size)
  {
    octet = 0x80 | ((ch >> 0) & 0x3F);
  }  
  else if(4 == size)
  {
    octet = 0x80 | ((ch >> 6) & 0x3F);
  }
  
  return octet;
}
char utf8_4th_byte_mask(unic_char ch, short size)
{
  char octet = 0x0;
  
  if(4 == size)
  {
    octet = 0x80 | ((ch >> 0) & 0x3F);
  }
  
  return octet;
}

short utf8_char_determ_num_otects(unic_char ch)
{
  if(ch >= 0x0 && ch < 0x80)
    return 1;
  else if(ch > 0x7F && ch < 0x800)
    return 2;
  else if(ch > 0x7FF && ch < 0xFFFF)
    return 3;
  else if(ch > 0x10000 && ch <= 0x10FFFF)
    return 4;
  else
    return -1;
}

utf8_char* utf8_encode(const unic_char ch)
{
  utf8_char *ret_ch = (utf8_char*)malloc(sizeof(utf8_char));
  int       size    = 0;

  ret_ch->size      = utf8_char_determ_num_otects(ch);
  ret_ch->octets    = (char*)malloc(sizeof(char)*ret_ch->size);
  
  size = ret_ch->size;  
  switch(ret_ch->size)
  {
    case 1:
      ret_ch->octets[0] = utf8_1st_byte_mask(ch, size);
      break;
    case 2:
      ret_ch->octets[0] = utf8_1st_byte_mask(ch, size);
      ret_ch->octets[1] = utf8_2nd_byte_mask(ch, size);      
      break;
    case 3:
      ret_ch->octets[0] = utf8_1st_byte_mask(ch, size);
      ret_ch->octets[1] = utf8_2nd_byte_mask(ch, size);
      ret_ch->octets[2] = utf8_3rd_byte_mask(ch, size);          
      break;
    case 4:
      ret_ch->octets[0] = utf8_1st_byte_mask(ch, size);
      ret_ch->octets[1] = utf8_2nd_byte_mask(ch, size);
      ret_ch->octets[2] = utf8_3rd_byte_mask(ch, size);          
      ret_ch->octets[3] = utf8_4th_byte_mask(ch, size);                
      break;
    default:
      break;
  }

  return ret_ch;
}

void print_utf8_char(const utf8_char *ch)
{
  short index = 0;
  for(;index < ch->size; index++)
  {
    printf("%#2x(%d) ", (ch->octets[index] & 0xFF), ch->octets[index]);
  }
  printf("\n");
}

utf8_char* utf8_from_chars(char *chs, short len)
{
  short index       = 0;
  utf8_char *ret_ch = (utf8_char*)malloc(sizeof(utf8_char));

  ret_ch->size   = len;
  ret_ch->octets = (char*)malloc(sizeof(char)*len);
  
  for(; index < len; index++)
  {
    ret_ch->octets[index] = chs[index];
  }
  
  return ret_ch;
}

short utf8_char_cmp(const utf8_char *ch1, const utf8_char *ch2)
{
  if(ch1->size != ch2->size)
    return 0;
  
  short index = 0;
  for(;index < ch1->size; index++)
  {
    //printf("%#2x %#2x\n", ch1->octets[index], ch2->octets[index]);
    if(ch1->octets[index] != ch2->octets[index])
      return 0;
  }
  
  return 1;
}

int main(int argc, char **argv)
{
  char      ch_Ko_Kai[] = {0xE0, 0xB8, 0x81};
  unic_char unic_ch     = 0x0041;
  utf8_char *utf8_ch    = utf8_encode(unic_ch);
  utf8_char *utf8_ch_A  = utf8_from_chars("A", 1);
  utf8_char *utf8_ch_B  = utf8_from_chars("B", 1);
  utf8_char *utf8_ch_Ko_Kai = utf8_from_chars(ch_Ko_Kai, 3);
    
  printf("size of unic_char is %d\n", sizeof(unic_char));

  print_utf8_char(utf8_ch);
  print_utf8_char(utf8_ch_A);

  if(!utf8_char_cmp(utf8_ch, utf8_ch_A))
  {
    printf("two utf8 characters mismatch!\n");
  }
  else
  {
    printf("two utf8 characters match\n");
  }
  
  unic_ch = 0x0E01; // Unicode for Thai character Ko Kai
  utf8_ch = utf8_encode(unic_ch);

  print_utf8_char(utf8_ch_Ko_Kai);  
  print_utf8_char(utf8_ch);
  
  if(!utf8_char_cmp(utf8_ch, utf8_ch_Ko_Kai))
  {
    printf("two utf8 characters mismatch!\n");
  }
  else
  {
    printf("two utf8 characters match\n");
  }  
}
