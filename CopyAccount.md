# lewis.github.io
Lewis's Github project
//code 

import ABSL;
import AP.FO.BusinessPartner.Global;
import AP.FO.BusinessPartnerRelationship.Global;

var vCust = Customer.Retrieve(this.BusinessPartnerUUID);
var CurrentCommon = vCust.CurrentCommon;
var Address = vCust.AddressInformation.Address;
var FirstLineName;
var SecondLineName;
var ThirdLineName;
var FourthLineName;
//客户状态为有效，且客户为正式客户是才能触发
if (vCust.CurrentBusinessCharacters.CustomerIndicator && (vCust.Status.LifeCycleStatusCode == "2"))
{
	//复制默认名称
	if (CurrentCommon.IsSet())
	{
		FirstLineName = CurrentCommon.Organisation.Name.FirstLineName;
		SecondLineName = CurrentCommon.Organisation.Name.SecondLineName;
		ThirdLineName = CurrentCommon.Organisation.Name.ThirdLineName;
		FourthLineName = CurrentCommon.Organisation.Name.FourthLineName;


		var CustomerRoot : elementsof Customer;
		CustomerRoot.CategoryCode = "2";//"1"
		var EU = Customer.Create(CustomerRoot);
		if (EU.IsSet())
		{

			//潜在客户Customer Indicator
			EU.CurrentBusinessCharacters.ProspectIndicator = true;
			EU.CurrentBusinessCharacters.CustomerIndicator = false;
			//组织信息
			EU.CurrentCommon.Organisation.Name.FirstLineName = FirstLineName;                
			EU.CurrentCommon.Organisation.Name.SecondLineName = SecondLineName;
			EU.CurrentCommon.Organisation.Name.ThirdLineName = ThirdLineName;
			EU.CurrentCommon.Organisation.Name.FourthLineName = FourthLineName;
			//激活EU
			EU.Activate();


			//复制客户国际版本名称
			var nameRoot : elementsof Customer.Common.Name;
			foreach (var name in CurrentCommon.Name.Where(n => n.RepresentationCode.content != "0"))
			{
				nameRoot.Clear();
				//国际版本代码
				nameRoot.RepresentationCode.content = name.RepresentationCode.content;
				//国际版本名称
				nameRoot.Organisation.Name.FirstLineName = name.Organisation.Name.FirstLineName;
				nameRoot.Organisation.Name.SecondLineName = name.Organisation.Name.SecondLineName;
				nameRoot.Organisation.Name.ThirdLineName = name.Organisation.Name.ThirdLineName;
				nameRoot.Organisation.Name.FourthLineName = name.Organisation.Name.FourthLineName;
				//创建国际版本名称		
				EU.CurrentCommon.Name.Create(nameRoot);
			}

			//记录新客户的C4C客户编号
			var EUID = EU.InternalID.RemoveLeadingZeros();
			//this.BusinessPartner.CurrentCommon.zQualityDemand = EUID;
			//复制ABC等级
			var ABCClassifications;
			if (vCust.ABCClassifications.IsSet())
			{
				ABCClassifications = vCust.ABCClassifications.CustomerABCClassificationCode.content;
				EU.ABCClassifications.Create().CustomerABCClassificationCode.content = ABCClassifications;
			}
    
			//客制字段
			EU.CurrentCommon.CustomerType = CurrentCommon.CustomerType;
			//数量
			EU.CurrentCommon.Quantity.content = CurrentCommon.Quantity.content;
			EU.CurrentCommon.Quantity.unitCode = CurrentCommon.Quantity.unitCode;
			//申请信用额度*(QY00)
			EU.CurrentCommon.ZAcc_Com_QY00_ApplicationCreditLimit.content = CurrentCommon.ZAcc_Com_QY00_ApplicationCreditLimit.content;
			EU.CurrentCommon.ZAcc_Com_QY00_ApplicationCreditLimit.currencyCode = CurrentCommon.ZAcc_Com_QY00_ApplicationCreditLimit.currencyCode;
			//佣金客户标志(QY00)
			EU.CurrentCommon.ZAcc_Com_QY00_CommissionCustomer = CurrentCommon.ZAcc_Com_QY00_CommissionCustomer;
			//批复信用额度*(QY00)
			EU.CurrentCommon.ZAcc_Com_QY00_CreditLimit.content = CurrentCommon.ZAcc_Com_QY00_CreditLimit.content;
			EU.CurrentCommon.ZAcc_Com_QY00_CreditLimit.currencyCode = CurrentCommon.ZAcc_Com_QY00_CreditLimit.currencyCode;
			//共享信用额度客户(QY00)
			EU.CurrentCommon.ZAcc_Com_QY00_CreditLimitShareCustomer = CurrentCommon.ZAcc_Com_QY00_CreditLimitShareCustomer;
			//信用等级*(QY00)
			EU.CurrentCommon.ZAcc_Com_QY00_CreditRank = CurrentCommon.ZAcc_Com_QY00_CreditRank;
			//信用调查日*(QY00)
			EU.CurrentCommon.ZAcc_Com_QY00_Creditinvestigationday = CurrentCommon.ZAcc_Com_QY00_Creditinvestigationday;
			//保证金额(QY00)
			EU.CurrentCommon.ZAcc_Com_QY00_Deposit.content = CurrentCommon.ZAcc_Com_QY00_Deposit.content;
			EU.CurrentCommon.ZAcc_Com_QY00_Deposit.currencyCode = CurrentCommon.ZAcc_Com_QY00_Deposit.currencyCode;
			//是否需要保证金(QY00)
			EU.CurrentCommon.ZAcc_Com_QY00_IsDepositCustomer = CurrentCommon.ZAcc_Com_QY00_IsDepositCustomer;
			//风险等级*(QY00)
			EU.CurrentCommon.ZAcc_Com_QY00_RiskRank = CurrentCommon.ZAcc_Com_QY00_RiskRank;
			//申请信用额度*(RH00)
			EU.CurrentCommon.ZAcc_Com_RH00_ApplicationCreditLimit.content = CurrentCommon.ZAcc_Com_RH00_ApplicationCreditLimit.content;
			EU.CurrentCommon.ZAcc_Com_RH00_ApplicationCreditLimit.currencyCode = CurrentCommon.ZAcc_Com_RH00_ApplicationCreditLimit.currencyCode;
			//佣金客户标志(RH00)
			EU.CurrentCommon.ZAcc_Com_RH00_CommissionCustomer = CurrentCommon.ZAcc_Com_RH00_CommissionCustomer;
			//批复信用额度*(RH00)
			EU.CurrentCommon.ZAcc_Com_RH00_CreditLimit.content = CurrentCommon.ZAcc_Com_RH00_CreditLimit.content;
			EU.CurrentCommon.ZAcc_Com_RH00_CreditLimit.currencyCode = CurrentCommon.ZAcc_Com_RH00_CreditLimit.currencyCode;
			//共享信用额度客户(RH00)
			EU.CurrentCommon.ZAcc_Com_RH00_CreditLimitShareCustomer = CurrentCommon.ZAcc_Com_RH00_CreditLimitShareCustomer;
			//信用等级*(RH00)
			EU.CurrentCommon.ZAcc_Com_RH00_CreditRank = CurrentCommon.ZAcc_Com_RH00_CreditRank;
			//信用调查日*(RH00)
			EU.CurrentCommon.ZAcc_Com_RH00_Creditinvestigationday = CurrentCommon.ZAcc_Com_RH00_Creditinvestigationday;
			//保证金额(RH00)
			EU.CurrentCommon.ZAcc_Com_RH00_Deposit.content = CurrentCommon.ZAcc_Com_RH00_Deposit.content;
			EU.CurrentCommon.ZAcc_Com_RH00_Deposit.currencyCode = CurrentCommon.ZAcc_Com_RH00_Deposit.currencyCode;
			//是否需要保证金(RH00)
			EU.CurrentCommon.ZAcc_Com_RH00_IsDepositCustomer = CurrentCommon.ZAcc_Com_RH00_IsDepositCustomer;
			//风险等级*(RH00)
			EU.CurrentCommon.ZAcc_Com_RH00_RiskRank = CurrentCommon.ZAcc_Com_RH00_RiskRank;
			//年度营业额
			EU.CurrentCommon.ZAcc_Gen_Annualturnover.content = CurrentCommon.ZAcc_Gen_Annualturnover.content;
			EU.CurrentCommon.ZAcc_Gen_Annualturnover.currencyCode = CurrentCommon.ZAcc_Gen_Annualturnover.currencyCode;
			//银行账号(外币不填)
			EU.CurrentCommon.ZAcc_Gen_BankAccount = CurrentCommon.ZAcc_Gen_BankAccount;
			//银行户主
			EU.CurrentCommon.ZAcc_Gen_BankAccountOwner = CurrentCommon.ZAcc_Gen_BankAccountOwner;
			//银行代码
			EU.CurrentCommon.ZAcc_Gen_BankCode = CurrentCommon.ZAcc_Gen_BankCode;
			//银行国家
			EU.CurrentCommon.ZAcc_Gen_BankCountry = CurrentCommon.ZAcc_Gen_BankCountry;
			//轴承代表型号
			EU.CurrentCommon.ZAcc_Gen_BearingModel = CurrentCommon.ZAcc_Gen_BearingModel;
			//工商注册有效期自
			EU.CurrentCommon.ZAcc_Gen_BusinessExpiryFrom = CurrentCommon.ZAcc_Gen_BusinessExpiryFrom;
			//工商注册有效期至
			EU.CurrentCommon.ZAcc_Gen_BusinessExpiryTo = CurrentCommon.ZAcc_Gen_BusinessExpiryTo;
			//客户类型
			EU.CurrentCommon.ZAcc_Gen_CompanyClassification = CurrentCommon.ZAcc_Gen_CompanyClassification;
			//客户企业集团
			EU.CurrentCommon.ZAcc_Gen_CompanyGroup = CurrentCommon.ZAcc_Gen_CompanyGroup;
			//客户来源
			EU.CurrentCommon.ZAcc_Gen_Customersource = CurrentCommon.ZAcc_Gen_Customersource;
			//客户战略
			EU.CurrentCommon.ZAcc_Gen_Customerstrategy = CurrentCommon.ZAcc_Gen_Customerstrategy;
			//年度轴承需求量
			EU.CurrentCommon.ZAcc_Gen_Demand.content = CurrentCommon.ZAcc_Gen_Demand.content;
			EU.CurrentCommon.ZAcc_Gen_Demand.currencyCode = CurrentCommon.ZAcc_Gen_Demand.currencyCode;
			//正式客户编号
			EU.CurrentCommon.ZAcc_Gen_ERPCustomerID = CurrentCommon.ZAcc_Gen_ERPCustomerID;
			//首次合同金额
			EU.CurrentCommon.ZAcc_Gen_FirstContractAmount.content = CurrentCommon.ZAcc_Gen_FirstContractAmount.content;
			EU.CurrentCommon.ZAcc_Gen_FirstContractAmount.currencyCode = CurrentCommon.ZAcc_Gen_FirstContractAmount.currencyCode;
			//首次合同交付时间
			EU.CurrentCommon.ZAcc_Gen_FirstContractTradeTime = CurrentCommon.ZAcc_Gen_FirstContractTradeTime;
			//年度NTN轴承需求量预测
			EU.CurrentCommon.ZAcc_Gen_Predictedamount.content = CurrentCommon.ZAcc_Gen_Predictedamount.content;
			EU.CurrentCommon.ZAcc_Gen_Predictedamount.currencyCode = CurrentCommon.ZAcc_Gen_Predictedamount.currencyCode;
			//营业利用率
			EU.CurrentCommon.ZAcc_Gen_ProfitRate = CurrentCommon.ZAcc_Gen_ProfitRate;
			//所属区域
			EU.CurrentCommon.ZAcc_Gen_Region = CurrentCommon.ZAcc_Gen_Region;
			//注册资金*
			EU.CurrentCommon.ZAcc_Gen_Registeredcapital.content = CurrentCommon.ZAcc_Gen_Registeredcapital.content;
			EU.CurrentCommon.ZAcc_Gen_Registeredcapital.currencyCode = CurrentCommon.ZAcc_Gen_Registeredcapital.currencyCode;
			//代表商种
			EU.CurrentCommon.ZAcc_Gen_Representativebusiness = CurrentCommon.ZAcc_Gen_Representativebusiness;
			//销售额
			EU.CurrentCommon.ZAcc_Gen_SalesAmount.content = CurrentCommon.ZAcc_Gen_SalesAmount.content;
			EU.CurrentCommon.ZAcc_Gen_SalesAmount.currencyCode = CurrentCommon.ZAcc_Gen_SalesAmount.currencyCode;
			//运输区域
			EU.CurrentCommon.ZAcc_Gen_ShipArea = CurrentCommon.ZAcc_Gen_ShipArea;
			//TQM开始年月
			EU.CurrentCommon.ZAcc_Gen_Startingyear = CurrentCommon.ZAcc_Gen_Startingyear;
			//在库金额
			EU.CurrentCommon.ZAcc_Gen_StoreAmount.content = CurrentCommon.ZAcc_Gen_StoreAmount.content;
			EU.CurrentCommon.ZAcc_Gen_StoreAmount.currencyCode = CurrentCommon.ZAcc_Gen_StoreAmount.currencyCode;
			//上下游关系
			EU.CurrentCommon.ZAcc_Gen_UpDownRelationship = CurrentCommon.ZAcc_Gen_UpDownRelationship;
			//员工数
			EU.CurrentCommon.ZAcc_Gen_employeecount = CurrentCommon.ZAcc_Gen_employeecount;
			//重要通知
			EU.CurrentCommon.ZAcc_Gen_importantNotice = CurrentCommon.ZAcc_Gen_importantNotice;
			//企业类型*
			EU.CurrentCommon.ZAcc_Gen_typeofenterprise = CurrentCommon.ZAcc_Gen_typeofenterprise;
			//客户背景
			EU.CurrentCommon.ZLead_AccountBackgroud = CurrentCommon.ZLead_AccountBackgroud;
			//主要产品
			EU.CurrentCommon.ZLead_MainProd = CurrentCommon.ZLead_MainProd;
			//轴承品牌
			EU.CurrentCommon.bearingBrand = CurrentCommon.bearingBrand;
			//轴承用途
			EU.CurrentCommon.bearingPurporse = CurrentCommon.bearingPurporse;
			//轴承类型
			EU.CurrentCommon.bearingType = CurrentCommon.bearingType;
			//主要竞争公司
			EU.CurrentCommon.competitorCompany = CurrentCommon.competitorCompany;
			//客户处理类型
			EU.CurrentCommon.dealType = CurrentCommon.dealType;
			//现地企业集团
			EU.CurrentCommon.localCompanyGroup = CurrentCommon.localCompanyGroup;
			//主要市场份额
			EU.CurrentCommon.marketAmount.content = CurrentCommon.marketAmount.content;
			EU.CurrentCommon.marketAmount.currencyCode = CurrentCommon.marketAmount.currencyCode; 
			//预计月销售金额
			EU.CurrentCommon.monthlySalesForecastAmount.content = CurrentCommon.monthlySalesForecastAmount.content;
			EU.CurrentCommon.monthlySalesForecastAmount.currencyCode = CurrentCommon.monthlySalesForecastAmount.currencyCode;
			//客户新企业集团
			EU.CurrentCommon.newCompanyGroup = CurrentCommon.newCompanyGroup;

		
			//当前客户的地址信息
			var addressInfo = vCust.AddressInformation;
			//复制地址
			foreach (var addItem in addressInfo.Address)
			{
				//新增地址
				var newAddressInfo = EU.AddressInformation.Create();
				//地址信息
				var newPostalAddress : elementsof Customer.AddressInformation.Address.PostalAddress;
				foreach (var item in addItem.PostalAddress)
				{
					newPostalAddress.Clear();
					//国际版本代码
					newPostalAddress.AddressRepresentationCode.content = item.AddressRepresentationCode.content;
					//国家
					newPostalAddress.CountryCode = addItem.DefaultPostalAddressRepresentation.CountryCode;				
					//区域
					newPostalAddress.RegionCode.content = addItem.DefaultPostalAddressRepresentation.RegionCode.content;				
					//城市
					newPostalAddress.CityName = addItem.DefaultPostalAddressRepresentation.CityName;				
					//区
					newPostalAddress.DistrictName = addItem.DefaultPostalAddressRepresentation.DistrictName;				
					//县
					newPostalAddress.CountyName = addItem.DefaultPostalAddressRepresentation.CountyName;			
					//街道
					newPostalAddress.StreetName = addItem.DefaultPostalAddressRepresentation.StreetName;				
					//地址行1
					newPostalAddress.StreetPrefixName = addItem.DefaultPostalAddressRepresentation.StreetPrefixName;				
					//地址行4
					newPostalAddress.StreetSuffixName = addItem.DefaultPostalAddressRepresentation.StreetSuffixName;			
					//地址行2
					newPostalAddress.AdditionalStreetPrefixName = addItem.DefaultPostalAddressRepresentation.AdditionalStreetPrefixName;			
					//地址行5
					newPostalAddress.AdditionalStreetPrefixName = addItem.DefaultPostalAddressRepresentation.AdditionalStreetSuffixName;
					//门牌号
					newPostalAddress.HouseID = addItem.DefaultPostalAddressRepresentation.HouseID;
					//楼号
					newPostalAddress.BuildingID = addItem.DefaultPostalAddressRepresentation.BuildingID;
					//转交人
					newPostalAddress.CareOfName = addItem.DefaultPostalAddressRepresentation.CareOfName;
					//楼层
					newPostalAddress.FloorID = addItem.DefaultPostalAddressRepresentation.FloorID;
					//房间号
					newPostalAddress.RoomID = addItem.DefaultPostalAddressRepresentation.RoomID;
					//邮政编码
					newPostalAddress.StreetPostalCode = addItem.DefaultPostalAddressRepresentation.StreetPostalCode;
					//新增国际版本地址
					newAddressInfo.Address.PostalAddress.Create(newPostalAddress);
				}
				
				//语言&首要联系方式
				var newCommunicationPreference = newAddressInfo.Address.CommunicationPreference.Create();
				if (addItem.CommunicationPreference.IsSet())
				{
					//对应语言
					newCommunicationPreference.CorrespondenceLanguageCode = addItem.CommunicationPreference.CorrespondenceLanguageCode;		
					newCommunicationPreference.PreferredCommunicationMediumTypeCode.content = addItem.CommunicationPreference.PreferredCommunicationMediumTypeCode.content;
				}
				//电话
				var newConventionalPhone = newAddressInfo.Address.DefaultConventionalPhone.Create();
				if (addItem.DefaultConventionalPhone.IsSet())
				{
					newConventionalPhone.FormattedNumberDescription = addItem.DefaultConventionalPhone.FormattedNumberDescription;
				}
				//手机
				var newMobilePhone = newAddressInfo.Address.DefaultMobilePhone.Create();
				if (addItem.DefaultMobilePhone.IsSet())
				{
					newMobilePhone.FormattedNumberDescription = addItem.DefaultMobilePhone.FormattedNumberDescription;	
				}
				//电子邮件
				var newEmail = newAddressInfo.Address.DefaultEMail.Create();
				if (addItem.DefaultEMail.IsSet())
				{
					newEmail.URI.content = addItem.DefaultEMail.URI.content;
				}
				//传真
				var newFacsimile = newAddressInfo.Address.DefaultFacsimile.Create();
				if (addItem.DefaultFacsimile.IsSet())
				{
					newFacsimile.FormattedNumberDescription = addItem.DefaultFacsimile.FormattedNumberDescription;
				}
				//网站
				var newWeb = newAddressInfo.Address.DefaultWeb.Create();
				if (addItem.DefaultWeb.IsSet())
				{
					newWeb.URI = addItem.DefaultWeb.URI;
				}
			}
			//复制销售数据
			//当前客户销售数据
			var salesData = vCust.SalesArrangement;
			foreach (var SDItem in salesData)
			{
				//为EU新增一条销售数据
				var newSalesData = SalesArrangement.CreateFromCustomer(EU);
				//销售组织UUID
				newSalesData.SalesOrganisationUUID.content = SDItem.SalesOrganisationUUID.content;
				//分销渠道
				newSalesData.DistributionChannelCode.content = SDItem.DistributionChannelCode.content;
				//分部
				newSalesData.DivisionCode.content = SDItem.DivisionCode.content;
				//货币&客户组
				var newPricingTerms = newSalesData.PricingTerms.Create();
				if (SDItem.PricingTerms.IsSet())
				{
					//货币
					newPricingTerms.CurrencyCode = SDItem.PricingTerms.CurrencyCode;
					//客户组
					newPricingTerms.CustomerGroupCode.content = SDItem.PricingTerms.CustomerGroupCode.content;
				}
				//交货信息
				var newDeliveryTerms = newSalesData.DeliveryTerms;
				if (SDItem.DeliveryTerms.IsSet())
				{
					//
					newDeliveryTerms.CompleteDeliveryRequestedIndicator = SDItem.DeliveryTerms.CompleteDeliveryRequestedIndicator;
					//交货优先权
					newDeliveryTerms.DeliveryPriorityCode = SDItem.DeliveryTerms.DeliveryPriorityCode;
					if (!SDItem.DeliveryTerms.Incoterms.ClassificationCode.IsInitial())
					{
						//国际贸易条款
						newDeliveryTerms.Incoterms.ClassificationCode = SDItem.DeliveryTerms.Incoterms.ClassificationCode;
					}
					if (!SDItem.DeliveryTerms.Incoterms.TransferLocationName.IsInitial())
					{
						//国际贸易条款地点
						newDeliveryTerms.Incoterms.TransferLocationName = SDItem.DeliveryTerms.Incoterms.TransferLocationName;
					}

				}
				//冻结理由
				var newBlockingReasons = newSalesData.BlockingReasons.Create();
				if (SDItem.BlockingReasons.IsSet())
				{
					//开票冻结
					newBlockingReasons.BillingBlockingReasonCode = SDItem.BlockingReasons.BillingBlockingReasonCode;
					//交货冻结
					newBlockingReasons.DeliveryBlockingReasonCode = SDItem.BlockingReasons.DeliveryBlockingReasonCode;
					//订单冻结
					newBlockingReasons.OrderBlockingReasonCode = SDItem.BlockingReasons.OrderBlockingReasonCode;
					//销售支持冻结
					newBlockingReasons.SalesSupportBlockingIndicator = SDItem.BlockingReasons.SalesSupportBlockingIndicator;
				}
				//付款条件
				var newPaymentTerms = newSalesData.PaymentTerms.Create();
				if (SDItem.PaymentTerms.IsSet())
				{
					newPaymentTerms.CashDiscountTermsCode.content = SDItem.PaymentTerms.CashDiscountTermsCode.content;
				}
				//业种信息
				newSalesData.ZSArr_BusinessID = SDItem.ZSArr_BusinessID.RemoveLeadingZeros();
				newSalesData.ZSArr_GIC = SDItem.ZSArr_GIC;
				newSalesData.ZSArr_GIC1 = SDItem.ZSArr_GIC1;
				newSalesData.ZSArr_GIC1EN = SDItem.ZSArr_GIC1EN;
				newSalesData.ZSArr_GICEN = SDItem.ZSArr_GICEN;
				newSalesData.ZSArr_NewBusinessCODE = SDItem.ZSArr_NewBusinessCODE;
				newSalesData.ZSArr_NewGICGIC1 = SDItem.ZSArr_NewGICGIC1;
				newSalesData.ZSArr_OldBusinessCODE = SDItem.ZSArr_OldBusinessCODE;
				newSalesData.ZSArr_OldGICGIC1 = SDItem.ZSArr_OldGICGIC1;
			}

			//复制BP关系
			//获取当前客户的关系
			var relationship = vCust.BusinessPartnerRelationship;
			var vCustUUID = vCust.UUID.content;
			var EUUUID = EU.UUID.content;

			foreach (var relation in relationship.Where(n=>n.CategoryCode.content == "BUR001"))
			{
				//定义关系根节点
				var relationRoot : elementsof BusinessPartnerRelationship;
				//关系代码
				relationRoot.CategoryCode.content = relation.CategoryCode.content;
				//关系角色1
				if (relation.FirstBusinessPartnerUUID.content == vCustUUID)
				{
					relationRoot.FirstBusinessPartnerUUID.content = EUUUID;			
				}
				else
				{
					relationRoot.FirstBusinessPartnerUUID.content = relation.FirstBusinessPartnerUUID.content;
				}
				//关系角色2
				if (relation.SecondBusinessPartnerUUID.content == vCustUUID)
				{
					relationRoot.SecondBusinessPartnerUUID.content = EUUUID;			
				}
				else
				{
					relationRoot.SecondBusinessPartnerUUID.content = relation.SecondBusinessPartnerUUID.content;
				}
				//是否主要
				relationRoot.DefaultIndicator = relation.DefaultIndicator;
				//销售组织
				relationRoot.SalesOrganisationUUID.content = relation.SalesOrganisationUUID.content;
				//分销渠道
				relationRoot.DistributionChannelCode.content = relation.DistributionChannelCode.content; 
				//部门
				relationRoot.DivisionCode.content = relation.DivisionCode.content;
				//创建关系
				BusinessPartnerRelationship.Create(relationRoot);
			}
			////创建客户跟EU的关系
			////定义关系根节点
			//var relationEU : elementsof BusinessPartnerRelationship;
			////关系代码
			//relationEU.CategoryCode.content = "ZBPR01";
			////关系角色1
			//relationEU.FirstBusinessPartnerUUID.content = vCustUUID;
			////关系角色2
			//relationEU.SecondBusinessPartnerUUID.content = EUUUID;
			////是否主要
			//relationEU.DefaultIndicator = false;
			////销售组织
			//relationEU.SalesOrganisationUUID.content = "";
			////分销渠道
			//relationEU.DistributionChannelCode.content = ""; 
			////部门
			//relationEU.DivisionCode.content = "";
			////创建关系
			//BusinessPartnerRelationship.Create(relationEU);



			//复制客户团队
			//获取当前客户的客户团队数据
			var employeeResponsible = vCust.CurrentEmployeeResponsible;
    
			foreach (var emp in employeeResponsible)
			{
				//新增一条客户团队数据
				var newEmployee = EU.CurrentEmployeeResponsible.Create();
				//角色代码
				newEmployee.PartyRoleCode = emp.PartyRoleCode;
				//员工ID
				newEmployee.EmployeeID.content = emp.EmployeeID.content;
				//是否主要
				newEmployee.DefaultIndicator = emp.DefaultIndicator;
				//销售组织
				newEmployee.SalesOrganisationUUID.content = emp.SalesOrganisationUUID.content;
				//分销渠道
				newEmployee.DistributionChannelCode.content = emp.DistributionChannelCode.content;
				//部门
				newEmployee.DivisionCode.content = emp.DivisionCode.content;
				//有效期自
				newEmployee.ValidityPeriod.StartDate = emp.ValidityPeriod.StartDate;
				//有效期至
				newEmployee.ValidityPeriod.EndDate = emp.ValidityPeriod.EndDate;
			}
		}
	}

}

//code closed


