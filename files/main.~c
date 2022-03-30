//---------------------------------------------------------------------------

#pragma hdrstop

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <libxml/xmlmemory.h>
#include <libxml/parser.h>

#define FILE_NAME_LEN   64
#define TLV_LEN_MAX     256
#define TLV_TYPE_HEX    0x00
#define TLV_TYPE_STR    0x01
//---------------------------------------------------------------------------

static void convert(FILE** xmlFile, FILE** tlvFile)
{
  xmlDocPtr doc;
  xmlNodePtr node;
  xmlChar *xmlStr;
  int param;
  char xmlFileName[FILE_NAME_LEN];
  char tlvFileName[FILE_NAME_LEN];
  char tlvBuff[TLV_LEN_MAX+2];
  int tlvBuffLen, cntNum, cntText;

  printf("Enter input xml-file name: ");
  if(!fgets(xmlFileName, FILE_NAME_LEN, stdin))
  {
    printf("Invalid file name!\n");
    return;
  }
  xmlFileName[strlen(xmlFileName)-1]=0;

  (*xmlFile)=fopen(xmlFileName,"r");
  if(!(*xmlFile))
  {
    printf("File %s not found\n",xmlFileName);
    return;
  }

  printf("Enter output tlv-file name: ");
  if(!fgets(tlvFileName, FILE_NAME_LEN, stdin))
  {
    printf("Invalid file name!\n");
    return;
  }
  tlvFileName[strlen(tlvFileName)-1]=0;

  *tlvFile=fopen(tlvFileName,"w");
  if(!(*tlvFile))
  {
    printf("File %s not found\n",tlvFileName);
    return;
  }

  doc = xmlParseFile(xmlFileName);
  if(doc == NULL)
  {
    printf("Converting error!\n");
    return;
  }

  node = xmlDocGetRootElement(doc);
  if(node == NULL)
  {
    printf("Error: no root element in the xml-file!\n");
    return;
  }

  cntNum=0;
  cntText=0;

  node = node->xmlChildrenNode;
  while(node != NULL)
  {
    if(!xmlStrcmp(node->name,(const xmlChar*)"text"))
    {
      xmlStr = xmlNodeListGetString(doc,node->xmlChildrenNode,1);
      if(xmlStr != NULL)
      {
        tlvBuffLen = strlen(xmlStr);
        if(tlvBuffLen > TLV_LEN_MAX)
          tlvBuffLen = TLV_LEN_MAX;

        tlvBuff[0] = TLV_TYPE_STR;
        tlvBuff[1] = tlvBuffLen;
        memcpy((void*)&tlvBuff[2],(void*)xmlStr,tlvBuffLen);

        fwrite(tlvBuff,tlvBuffLen+2,1,(*tlvFile));
        cntText++;
      }
    }
    else if(!xmlStrcmp(node->name,(const xmlChar*)"numeric"))
    {
      xmlStr = xmlNodeListGetString(doc,node->xmlChildrenNode,1);
      if(xmlStr != NULL)
      {
        param=atoi(xmlStr);

        tlvBuffLen = 4;
        tlvBuff[0] = TLV_TYPE_HEX;
        tlvBuff[1] = tlvBuffLen;
        tlvBuff[2] = (param >> 24) & 0xff;
        tlvBuff[3] = (param >> 16) & 0xff;
        tlvBuff[4] = (param >> 8) & 0xff;
        tlvBuff[5] = param & 0xff;

        fwrite(tlvBuff,tlvBuffLen+2,1,(*tlvFile));
        cntNum++;
      }
    }
    node = node->next;
  }
  printf("Convertion done:\n");
  printf(" - text: %d\n",cntText);
  printf(" - num: %d\n",cntNum);
}

int main(void)
{
  FILE* xmlFile;
  FILE* tlvFile;

  printf("\n");
  printf("Convert xml-file to tlv-file.\n");

  convert(&xmlFile,&tlvFile);

  if(xmlFile != NULL)
    fclose(xmlFile);

  if(tlvFile != NULL)
    fclose(tlvFile);

  printf("Press ENTER to quit...");
  while(getchar() != '\n');

  return 0;
}
//---------------------------------------------------------------------------
