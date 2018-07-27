# lewis.github.io
Lewis's Github project
/*W code*/
//create XBO 
import AP.Common.GDT  as apCommonGDT;
import AP.CRM.Global;
import AP.FO.BusinessPartner.Global;

[Extension] businessobject AP.CRM.Global:Lead raises LeadErr {
 
        // You must activate this business object before you can access the extension fields
        // or messages in script files, forms, and screens.
		[Label("事业CODE")] element ZLead_BusinessCODE:LANGUAGEINDEPENDENT_EXTENDED_Text;

		[Label("事业C")] element ZLead_BusinessCODE1:LANGUAGEINDEPENDENT_EXTENDED_Text;

		[Label("事业CO")] element ZLead_BusinessCODE2:LANGUAGEINDEPENDENT_EXTENDED_Text;

		[Label("客户编码")] element ZAgain_ID_:BusinessPartnerInternalID;

		message LeadErr text "&1": apCommonGDT:LANGUAGEINDEPENDENT_EXTENDED_Text;
    
   		node Party {
   	    } 
    
   		node Item {

   	    } 
     
	 action LeadTest;

	 action AddOwner;
  
}
*/

//Create ABSL
import ABSL;
import AP.Common.GDT as apCommonGDT;
import AP.CRM.Global;
import AP.FO.BusinessPartner.Global as BPGlobal;
import AP.FO.MOM.Global;

var leadOB = this.GetFirst();
if(leadOB.ZAgain_ID_ != ""){
	var cus = Customer.Retrieve(leadOB.ZAgain_ID_);
	if(cus.IsSet()){
		leadOB.ZLead_BusinessCODE = cus.Common.GetFirst().BusinessPartnerFormattedName;
	}
}

#1 增强TI界面新字段
#2 改变字段ID属性
#3 扩展scenario
#4 增加XBO的scenario的名称及code
#5 Lead选择的代理商，数据写入关系
    if 代理商客户不为空
    role=固定
    客户=代理商角色
    销售组织=Account 销售组织
    分销渠道=Account 分销渠道
    货币，Lead没有货币；
    else 不写入

