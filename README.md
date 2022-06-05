# IrishRail

<img width="1680" alt="image" src="https://user-images.githubusercontent.com/33503189/169656821-a2ab3130-37a2-418a-8ffd-a3f957e17f92.png">

```
DECLARE
    l_envelope CLOB;
    l_xml      XMLTYPE;
BEGIN
    -- reference http://api.irishrail.ie/realtime/realtime.asmx?op=getStationDataByCodeXML_WithNumMins
    -- The following is a sample SOAP 1.1 request and response
    -- getStationDataByCodeXML_WithNumMins
    l_envelope := '<?xml version="1.0" encoding="utf-8"?>
    <soap:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
    <soap:Body>
        <getStationDataByCodeXML_WithNumMins xmlns="http://api.irishrail.ie/realtime/">
        <StationCode>'||:P1_STATIONCODE||'</StationCode>
        <NumMins>'||:P1_NUMMINS||'</NumMins>
        </getStationDataByCodeXML_WithNumMins>
    </soap:Body>
    </soap:Envelope>';
    
    -- request
    l_xml := apex_web_service.make_request(
        p_url => 'https://api.irishrail.ie/realtime/realtime.asmx',
        p_action => 'http://api.irishrail.ie/realtime/getStationDataByCodeXML_WithNumMins',
        p_envelope => l_envelope
    );

    -- create a collection
    apex_collection.create_or_truncate_collection(
        p_collection_name => 'irishrail');

    -- save response xml to collection for query later   
    apex_collection.add_member(
        p_collection_name => 'irishrail',
        p_xmltype001      => l_xml);

    -- htp.p('l_xml=' || l_xml.getClobVal());
END;
```

<img width="1680" alt="image" src="https://user-images.githubusercontent.com/33503189/169656854-e89eefe0-d0f3-4fed-b02c-bf4061806a1b.png">

<img width="1473" alt="image" src="https://user-images.githubusercontent.com/33503189/169656790-b225252a-e7aa-48d3-9eb9-75ace7b4532e.png">


Try:  https://apex.oracle.com/pls/apex/r/mucs/irishrail/home
