# ZohoCrm_MailMerge
This script automates the process of uploading a Mail Merge document to specific fields in a Zoho CRM module. It utilizes Zoho CRM API for seamless integration and document handling.

Features:
✅ Uploads mail merge documents to custom fields in Zoho CRM
✅ Uses Zoho CRM API for authentication and data transfer

Usage:
Configure your Zoho API credentials.
Specify the CRM module and field where the document should be uploaded.
Run the script to upload your Mail Merge document.
Code:
caseID = cid.toString();
lead_mail_merge_template_resp = invokeurl
[
	url :"https://www.zohoapis.com/crm/v2/settings/templates?type=mailmerge&module=Deals"
	type :GET
	connection:"crm"
];
templates = lead_mail_merge_template_resp.get("templates");
updateMap = Map();
counter = 0;
for each  temp_rec in templates
{
	temp_id = temp_rec.get("id");
	temp_name = temp_rec.get("name");
	map = {"download_mail_merge":{{"mail_merge_template":{"id":temp_id},"output_format":"pdf","file_name":temp_name + " - " + cid}}};
	merge = invokeurl
	[
		url :"https://www.zohoapis.com/crm/v7/Deals/" + cid + "/actions/download_mail_merge"
		type :POST
		parameters:map.toString()
		connection:"crm"
	];
	merge.setParamName("file");
	info merge;
	zfs = invokeurl
	[
		url :"https://www.zohoapis.com/crm/v7/files"
		type :POST
		files:merge
		connection:"crm"
	];
	info zfs;
	filename = zfs.get("data").get(0).get("details").get("name");
	fileId = zfs.get("data").get(0).get("details").get("id");
	if(filename == "Late_fee_Waiver_.zdoc_-_6632557000000627051.pdf")
	{
		fmp = Map();
		fmp.put("file_id",fileId);
		fileList = List();
		fileList.add(fmp);
		updateMap.put("Late_Fee_Waiver",fileList);
	}
	if(filename == "DSS_8F-E.zdoc_-_6632557000000627051.pdf")
	{
		fmp2 = Map();
		fmp2.put("file_id",fileId);
		fileList2 = List();
		fileList2.add(fmp2);
		updateMap.put("DSS_8f_e",fileList2);
	}
	if(filename == "W_147N.zdoc_-_6632557000000627051.pdf")
	{
		fmp7 = Map();
		fmp7.put("file_id",fileId);
		fileList7 = List();
		fileList7.add(fmp7);
		updateMap.put("W_147N_Security_Voucher",fileList7);
	}
	if(filename == "HRA.zdoc_-_6632557000000627051.pdf")
	{
		fmp5 = Map();
		fmp5.put("file_id",fileId);
		fileList5 = List();
		fileList5.add(fmp5);
		updateMap.put("HRA_121_Request_Fee",fileList5);
	}
	if(filename == "W_147JJ.zdoc_-_6632557000000627051.pdf")
	{
		fmp6 = Map();
		fmp6.put("file_id",fileId);
		fileList6 = List();
		fileList6.add(fmp6);
		updateMap.put("W_147_JJ_Request_Fee",fileList6);
	}
}
optionalMap = Map();
update_rec = zoho.crm.updateRecord("Deals",cid,updateMap,optionalMap);
info update_rec;
