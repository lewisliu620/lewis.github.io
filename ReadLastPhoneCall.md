# lewis.github.io
Lewis's Github project

//代码ABAL开始

import ABSL;
var activityType = this.TypeCode;
if(activityType == "86"){
	//TQM客户
	var prospect = this.MainActivityParty.Party.BusinessPartner;	

	//特定事項
	var ZPhoneCall_DecisionMatter = this.ZAcc_Gen_Decision;
	//一周活动的回顾
	var ZPhoneCall_OneWeekActivity = this.ZAcc_Gen_OneWeekActivity;
	//上周的决定事项
	var ZPhoneCall_OneWeekDecide = this.ZAcc_Gen_OneWeekDecide;

	if(prospect.IsSet()){
		//客户名称
		var prospectName = prospect.CurrentCommon.Organisation.Name.FirstLineName;
		//根据客户查询最新的电话
		var q = Activity.QueryByElements;
		var s = q.CreateSelectionParams();
		s.Add(q.PartyID,"I","EQ",prospect.InternalID);
		var r = q.Execute(s);
		//为TQM的活动
		var p = r.Where(n=>n.TypeCode == "86");
		//TQM按照创建时间排序
		p.OrderBy(n=>n.CreationDate);
		//判断是否存在活动
		if(p.GetLast().IsSet()){
		var lastTqmID = p.GetLast().ID.RemoveLeadingZeros();
		//判断当前TQM是否最新的TQM
		if(lastTqmID == this.ID.RemoveLeadingZeros()){
			//上周的决定事项
			prospect.CurrentCommon.ZAcc_Gen_OneWeekDecide = ZPhoneCall_OneWeekDecide;
			//一周活动的回顾
			prospect.CurrentCommon.ZAct_Gen_OneWeekActivity = ZPhoneCall_OneWeekActivity;
			//特定事項
			prospect.CurrentCommon.ZAcc_Gen_SpecDecide = ZPhoneCall_DecisionMatter;

			//检查是否成功更新值
			if((prospect.CurrentCommon.ZAcc_Gen_OneWeekDecide == ZPhoneCall_OneWeekDecide)
			&&(prospect.CurrentCommon.ZAct_Gen_OneWeekActivity == ZPhoneCall_OneWeekActivity)
			&&(prospect.CurrentCommon.ZAcc_Gen_SpecDecide == ZPhoneCall_DecisionMatter)){
				raise ActMsg.Create("I","字段已更新到客户");
			}else{
				raise ActMsg.Create("E","客户"+prospectName+"被锁定，未能更新到客户");				
			}
		}
		}
	}
}







