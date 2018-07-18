# MIRTH-Repository
MIRTH code repository
try {channelMap.put('hospital', '1');} catch (e) { } 
	
try { channelMap.put('instance1', (msg['PID']['PID.3'][0]['PID.3.1'].toString())); } catch (e) { }
	
try { channelMap.put('instance2', (msg['PID'][0]['PID.18']['PID.18.1'].toString())); } catch (e) { }
	
try { channelMap.put('instance3', (msg['PID'][0]['PID.18']['PID.18.1'].toString())); } catch (e) { }

try { channelMap.put('instance5', (msg['PID'][0]['PID.18']['PID.18.1'].toString())); } catch (e) { }
	
try { channelMap.put('message_id', (msg['MSH'][0]['MSH.10']['MSH.10.1'].toString())); } catch (e) { }

try { channelMap.put('gender', (msg['PID']['PID.8']['PID.8.1'].toString())); } catch (e) { }
	
//age in months
try { 
var dob = msg['PID']['PID.7']['PID.7.1'].toString();
      
     var year = dob.substring(0, 4); 
     var month = dob.substring(4, 6); //January is 0 for javascript 
     var day = dob.substring(6, 8);

var nowDateString = DateUtil.getCurrentDate('yyyyMMdd'); 
var nowYear = nowDateString.substring(0, 4);
     var nowMonth = nowDateString.substring(4, 6); //January is 0 for javascript 
     var nowDay = nowDateString.substring(6, 8);

var YrsDiff = nowYear - year; 

if (nowMonth > month) { 
var MnthDiff = nowMonth - month;
} else if (nowMonth < month) { 
var MnthDiff = 12 - (month - nowMonth); 
YrsDiff--; 
} else { 
var MnthDiff = 0; 
    } 

var res = (12 * YrsDiff) + MnthDiff; 

} catch(e) { 
channelMap.put('err', e); 
} 

channelMap.put('age', res);

//payor
var payor;
try{ payor = msg['IN1'][0]['IN1.4']['IN1.4.1'].toString();} catch (e) { }

if ( payor != ''){
	channelMap.put('payor', payor);
}

else {
	channelMap.put('payor', "NULL VALUE");
	}

//payor type
var payor_type;

try { payor_type = msg['PV1'][0]['PV1.20']['PV1.20.1'].toString();} catch (e) { }

if (payor_type != ''){
       channelMap.put('payor_type', payor_type);
}

else {
	channelMap.put('payor_type', "NULL VALUE");
	}

//chief complaint
var chief_complaint;
var diag_code;

try {
	chief_complaint = msg['DG1'][0]['DG1.4']['DG1.4.1'].toString().substr(0,128);
	diag_code = msg['DG1'][0]['DG1.2']['DG1.2.1'].toString();
} catch (e) {}

if(diag_code == "" ) { 
channelMap.put('chief_complaint', chief_complaint); 
}

else {
	channelMap.put('chief_complaint', "NULL VALUE");
}

//chief compLIAnt free spelled wron intentionally
var chief_compliant_free;
var diag_code;

try {
	chief_compliant_free = msg['DG1'][0]['DG1.4']['DG1.4.1'].toString().substr(0,128);
	diag_code = msg['DG1'][0]['DG1.2']['DG1.2.1'].toString();
} catch (e) {}

if(diag_code == "" ) { 
channelMap.put('chief_compliant_free', chief_compliant_free); 
}

else {
	channelMap.put('chief_compliant_free', "NULL VALUE");
}

//discharge diagnosis
var discharge_diagnosis;
var diag_code;

try {
	discharge_diagnosis = msg['DG1'][0]['DG1.4']['DG1.4.1'].toString().substr(0,128);
	diag_code = msg['DG1'][0]['DG1.2']['DG1.2.1'].toString();
} catch (e) {}

if(diag_code != "" ) { 
channelMap.put('discharge_diagnosis', discharge_diagnosis); 
}

else {
	channelMap.put('discharge_diagnosis', "NULL VALUE");
}

//location
var current_department = msg['PV1'][0]['PV1.3']['PV1.3.1'].toString();
var current_room = msg['PV1'][0]['PV1.3']['PV1.3.2'].toString();
var department_blacklist1 = "MER";

if ((current_department == department_blacklist1) && (current_room.length > 0)) {
	channelMap.put('location', current_room);
} else {
	channelMap.put('location', "NULL VALUE");
	}

//first location
var current_department = msg['PV1']['PV1.3']['PV1.3.1'].toString();
var current_room = msg['PV1']['PV1.3']['PV1.3.2'].toString();
var event = msg['MSH']['MSH.9']['MSH.9.2'].toString();
var department_blacklist1 = "MER";

// We'll parse location on all message types, as long as patient is still in ED
if ((current_department == department_blacklist1) && (current_room.length > 0) && (event == "A01")) {
	channelMap.put('first_location', current_room);
} else {
	channelMap.put('first_location', "NULL VALUE");
	}

//care area name
var care_area_name;

try {
	care_area_name = msg['PV1']['PV1.3']['PV1.3.3'].toString();
} catch (e) {}

if(care_area_name != '' ) { 
channelMap.put('care_area_name', care_area_name); 
}

else {
	channelMap.put('care_area_name', "NULL VALUE");
}

var value = 'NULL VALUE';

//admitting_unit, bed_destination and admit_service
var trigger_event = msg['MSH']['MSH.9']['MSH.9.2'].toString();
var point_of_care = msg['PV1']['PV1.3']['PV1.3.1'].toString();
var bed_destination = msg['PV1']['PV1.3']['PV1.3.2'].toString();
var previous_department = msg['PV1']['PV1.6']['PV1.6.1'].toString();
var department_blacklist1 = "MER";
var value = 'NULL VALUE';
var hsv_val = msg['PV1']['PV1.10']['PV1.10.1'].toString();
var hsv_mapped = "NULL VALUE";

if ( hsv_val.length > 0) {
	channelMap.put('admit_service', hsv_val.substr(0,32));
	hsv_mapped = hsvmap(hsv_val);
	channelMap.put('admit_service', hsv_mapped);
}

// If the patient gets transferred to a non-ED location, coming from the ED, then that must be their admitting unit
if (( trigger_event == 'A02') && (point_of_care != department_blacklist1) && ( previous_department == department_blacklist1)){
	channelMap.put('admitting_unit', point_of_care);
	channelMap.put('bed_destination', bed_destination);
	channelMap.put('admit_service', hsv_mapped);
	}

else {
	channelMap.put('admitting_unit', value);
	channelMap.put('bed_destination', value);
	channelMap.put('admit_service', value);
}

//discharge_disposition, _map and disposition_selected time
var trigger_event = msg['MSH']['MSH.9']['MSH.9.2'].toString();
var message_time = msg['MSH']['MSH.7']['MSH.7.1'].toString();
var discharge_val = msg['PV1']['PV1.36']['PV1.36.1'].toString();
var current_department = msg['PV1']['PV1.3']['PV1.3.1'].toString();
var ed_service = msg['PV1']['PV1.10']['PV1.10.1'].toString();
var pat_status = msg['PV1']['PV1.2']['PV1.2.1'].toString();
var department_blacklist1 = "MER";
var dispo_mapped;
var value = 'NULL VALUE';

try {
	icd_10 = msg['DG1'][0]['DG1.2']['DG1.2.1'].toString();
} catch (e) {}

// Set disposition selected to Null Value
channelMap.put('disposition_selected', "NULL VALUE")
try{zpv_field = msg['ZPV']['ZPV.15']['ZPV.15.1'].toString();} catch (e) {}

// if ed_service is NOT EDS consider admitted
if (( current_department != department_blacklist1) || (pat_status == 'I' || pat_status == 'V' ))  {
	channelMap.put('discharge_disposition', "ADMITTED");
	channelMap.put('discharge_disposition_map', "admitted");
	channelMap.put('disposition_selected', message_time);

}

// If we see an A03 then the patient is discharged
else if ( trigger_event == 'A03') {
	channelMap.put('discharge_disposition', discharge_val.substr(0,32));
	dispo_mapped = dispomap(discharge_val);
	channelMap.put('discharge_disposition_map', dispo_mapped);
	//channelMap.put('disposition_selected', message_time);
}
/* 
Commenting this out - an A14 is treated as a pending arrival to the ED
If we see a pending admit, then write in admitted
else if ( trigger_event == 'A14') {
	channelMap.put('discharge_disposition', "ADMITTED");
	channelMap.put('discharge_disposition_map', "admitted");
	channelMap.put('disposition_selected', message_time);
}
*/
// If we see a pending discharge, then write in discharged
else if ( trigger_event == 'A08' && zpv_field == 'Discharge') {
	channelMap.put('discharge_disposition', "Discharge");
	channelMap.put('discharge_disposition_map', "discharged");
	channelMap.put('disposition_selected', message_time);
}

else {
	channelMap.put('discharge_disposition', value);
	channelMap.put('discharge_disposition_map', value);
	channelMap.put('disposition_selected', value);
}

//patient class
try{
var patient_class = msg['PV1']['PV1.2']['PV1.2.1'].toString();
channelMap.put('patient_class', patient_class);
} catch (e) { }

var value = 'NULL VALUE';

// Parse out the operator name following our convention
var operator_name = (
	msg['EVN']['EVN.5']['EVN.5.2'].toString() + ", "+
	msg['EVN']['EVN.5']['EVN.5.3'].toString() + " " +
	msg['EVN']['EVN.5']['EVN.5.4'].toString() + " " +
	msg['EVN']['EVN.5']['EVN.5.5'].toString() + " " +
	msg['EVN']['EVN.5']['EVN.5.6'].toString() + " " +
	msg['EVN']['EVN.5']['EVN.5.7'].toString() + " "
	)
// Clean up whitespace if any
operator_name = operator_name.trim()

if (( msg['EVN']['EVN.5']['EVN.5.2'].toString() == 'HM INTERFACE') || (msg['EVN']['EVN.5']['EVN.5.2'].toString() == 'CR')){
	channelMap.put('primary_nurse', 'NULL VALUE');
} else {
	channelMap.put('primary_nurse', operator_name);
}

// Handle null values
      channelMap.put('triage_complete', value);
      channelMap.put('acuity_assigned', value);
      channelMap.put('triage_nurse', value);
      channelMap.put('acuity', value);
var acuity_found = 0;
	var height;
	var ft;
	var tick;
	var quote;
	var inches;
	var weight;
	var found_ht = 0;
	var found_wt = 0;
	var accuity_found = 0;
	
for each(obx in msg.OBX) {

	if(obx['OBX.3']['OBX.3.1'].toString()=='HT'){
		found_ht = 1;
		height = obx['OBX.5']['OBX.5.1'].toString();
		channelMap.put('height', height);
	} else if(obx['OBX.3']['OBX.3.1'].toString()=='WT'){
		found_wt = 1;
		weight = obx['OBX.5']['OBX.5.1'].toString();
		channelMap.put('weight', weight);
	} else if(obx['OBX.3']['OBX.3.1'].toString()== "ACU") {
      channelMap.put('acuity', obx['OBX.5']['OBX.5.1'].toString());

      // We will write in the event time to capture when Acuity was assigned.
      // Our coalesce statement will not allow this to be overwritten once written once
      // We are assuming triage is complete once acuity is assigned
      //channelMap.put('triage_complete', msg['MSH']['MSH.7']['MSH.7.1'].toString());
      channelMap.put('acuity_assigned', msg['MSH']['MSH.7']['MSH.7.1'].toString());

      // We will write in the operator name to capture who assigned the Acuity.
      // Our coalesce statement will not allow this to be overwritten once written once
      // We are assuming that when an acuity is assigned, it's done by the triage nurse
      //channelMap.put('triage_nurse', operator_name);
      acuity_found++;
      }
}
if(found_ht < 1){ channelMap.put('height', 'NULL VALUE');}
if(found_wt < 1){ channelMap.put('weight', 'NULL VALUE');}
if(acuity_found < 1){
	 channelMap.put('acuity','NULL VALUE');
	 //channelMap.put('triage_complete', 'NULL VALUE');
      channelMap.put('acuity_assigned', 'NULL VALUE');
}

// triage_complete, roomed_in_ed and triage_nurse
var trigger_event = msg['MSH']['MSH.9']['MSH.9.2'].toString();
var current_room = msg['PV1']['PV1.3']['PV1.3.2'].toString();
var roomed_in_ed = msg['PV1']['PV1.44']['PV1.44.1'].toString();
var value = 'NULL VALUE';

if (current_room == 'EDT' ) {
	channelMap.put('triage_complete', msg['MSH']['MSH.7']['MSH.7.1'].toString());
	channelMap.put('roomed_in_ed', value);
	channelMap.put('triage_nurse', operator_name);
}

else if (current_room == '' ) {
	channelMap.put('roomed_in_ed', value);
}

else {
	channelMap.put('roomed_in_ed', roomed_in_ed);
}

/*}

if ((trigger_event == 'A01') || (trigger_event == 'A02') && (current_room != "EDT" )) {
	channelMap.put('roomed_in_ed', roomed_in_ed);
}

else if ((trigger_event != 'A01') && (current_room != "EDT" )) {
	channelMap.put('roomed_in_ed', value);
}*/

/*var trigger_event = msg['MSH']['MSH.9']['MSH.9.2'].toString();
var roomed_in_ed = msg['PV1']['PV1.44']['PV1.44.1'].toString();
var value = 'NULL VALUE';

if ( trigger_event == 'A01') {
	channelMap.put('roomed_in_ed', roomed_in_ed);
}

if ( trigger_event != 'A01') {
	channelMap.put('roomed_in_ed', value);
}*/

// provider, first_provider, md_start, md_assign and admit_doc
var provider;
var first_provider;
var md_start;
var md_assign;
try {provider = msg['PV1']['PV1.7'][0]['PV1.7.1'].toString();}catch (e){logger.error('Error 1'); 
	channelMap.put('provider', 'NULL VALUE');
	channelMap.put('first_provider', 'NULL VALUE');
	channelMap.put('md_start', 'NULL VALUE');
	channelMap.put('md_assign', 'NULL VALUE');}
try {
if (provider.length > 0 ){
	channelMap.put('provider', (msg['PV1']['PV1.7'][0]['PV1.7.2'].toString() + ', ' + msg['PV1']['PV1.7'][0]['PV1.7.3'].toString()));
	channelMap.put('first_provider', (msg['PV1']['PV1.7'][0]['PV1.7.2'].toString() + ', ' + msg['PV1']['PV1.7'][0]['PV1.7.3'].toString()));
	channelMap.put('md_start', msg['MSH']['MSH.7']['MSH.7.1'].toString());
	channelMap.put('md_assign', msg['MSH']['MSH.7']['MSH.7.1'].toString());
} 
else {
	channelMap.put('provider', 'NULL VALUE');
	channelMap.put('first_provider', 'NULL VALUE');
	channelMap.put('md_start', 'NULL VALUE');
	channelMap.put('md_assign', 'NULL VALUE');
}
 if (patient_class == 'I' && provider.length > 0){
 	channelMap.put('admit_doc', provider);
 } else {
 	channelMap.put('admit_doc', 'NULL VALUE');
 }
 }catch (e) {logger.error('Error 2');
 	channelMap.put('provider', 'NULL VALUE');
	channelMap.put('first_provider', 'NULL VALUE');
	channelMap.put('md_start', 'NULL VALUE');
	channelMap.put('md_assign', 'NULL VALUE');
	}
	
 
 //attending
try { var name_last = msg['PV1']['PV1.7'][0]['PV1.7.3'].toString();} catch (e) { }
try { var attending = msg['PV1']['PV1.7'][0]['PV1.7.2'].toString() + ", " + msg['PV1']['PV1.7'][0]['PV1.7.3'].toString() + " " + msg['PV1']['PV1.7'][0]['PV1.7.4'].toString();} catch (e) { }
if ( name_last != '') {
	channelMap.put('attending', attending);
}

else {
	channelMap.put('attending', value);
	}

/*var trigger_event = msg['MSH']['MSH.9']['MSH.9.2'].toString();
var check_in_time = msg['MSH']['MSH.7']['MSH.7.1'].toString();//was PV2-50.1 but no PV2 segment being sent
var patient_class = msg['PV1']['PV1.3']['PV1.3.1'].toString();
var value = 'NULL VALUE';

if (trigger_event.match("A04|A05") && (check_in_time != '')) {
	channelMap.put('check_in_time', check_in_time);
}
else {
	channelMap.put('check_in_time', value);
}*/

//check in time
var check_in_time;
try {check_in_time = msg['PV1']['PV1.44']['PV1.44.1'].toString();} catch(e) {}
if (check_in_time.length > 0){
	channelMap.put('check_in_time', check_in_time);
} else {
	channelMap.put('check_in_time', 'NULL VALUE');
}

//check out time
var trigger_event = msg['MSH']['MSH.9']['MSH.9.2'].toString();
var check_out_timea02 = msg['MSH']['MSH.7']['MSH.7.1'].toString();
var check_out_timea03 = msg['PV1']['PV1.45']['PV1.45.1'].toString();
var check_out_timea06 = msg['PV1']['PV1.44']['PV1.44.1'].toString();
var current_department = msg['PV1']['PV1.3']['PV1.3.1'].toString();
var current_room = msg['PV1']['PV1.3']['PV1.3.2'].toString();
var department_blacklist1 = "MER";
var value = 'NULL VALUE';

// Note, we need to change the department name for othe facilities
if (( trigger_event.match("A02|A11") && ( current_department != department_blacklist1))) {
	channelMap.put('check_out_time', check_out_timea02);
}

else if ( trigger_event == 'A03') {
	channelMap.put('check_out_time', check_out_timea03);
}

else if (( trigger_event == 'A06') && ( current_department != department_blacklist1)) {
	channelMap.put('check_out_time', check_out_timea06);
}

else { 
	channelMap.put('check_out_time', value);
}

//mental_health_flag, ip_op_obs, outlier, final_destination, final_departure,quick_reg,to_waiting_room ect......
channelMap.put('triage_start', "NULL VALUE");
channelMap.put('mental_health_flag', "NULL VALUE");
channelMap.put('ip_op_obs', "NULL VALUE");
channelMap.put('outlier', "0");
channelMap.put('outlier_note', "NULL VALUE");
channelMap.put('final_destination', "NULL VALUE");
channelMap.put('final_departure', "NULL VALUE");
channelMap.put('quick_reg', "NULL VALUE");
channelMap.put('to_waiting_room', "NULL VALUE");
channelMap.put('nurse_assessment_end', "NULL VALUE");
channelMap.put('fasttrack_start', "NULL VALUE");
channelMap.put('fasttrack_discharge', "NULL VALUE");
channelMap.put('registration_start', "NULL VALUE");
channelMap.put('registration_complete', "NULL VALUE");
channelMap.put('ready_for_reeval', "NULL VALUE");
channelMap.put('consult_called', "NULL VALUE");
channelMap.put('consult_arrived', "NULL VALUE");
channelMap.put('bed_requested', "NULL VALUE");
channelMap.put('bed_ready', "NULL VALUE");
channelMap.put('rn_admit_handoff', "NULL VALUE");
channelMap.put('ready_for_admission', "NULL VALUE");
channelMap.put('ready_for_discharge', "NULL VALUE");
channelMap.put('ready_for_transfer', "NULL VALUE");
channelMap.put('porter_called', "NULL VALUE");
channelMap.put('board_time', "NULL VALUE");
channelMap.put('bridge_order', "NULL VALUE");

//means of arrival
try{
var means_of_arrival = msg['PV2']['PV2.38']['PV2.38.1'].toString();
channelMap.put('means_of_arrival', means_of_arrival);
} catch (e) { }

//means of arrival mapped
var means_of_arrival = msg['PV2']['PV2.38']['PV2.38.1'].toString();
var arrival_val;
var means_of_arrival_raw;
var arrival_mapped = '';

var arrival_val_mapped = {
	"Wheelchair":	"self",
	"Taxie":  "self",
	"Public Transportation":  "self",
	"Police":  "ems",
	"Other":  "other",
	"NULL":  "other",
	"Medical Flight":	"flight",
	"Hospital Transport":	"ems",
	"Car":	"self",
	"Assist From Vehicle":  "self",
	"Ambulance": "ems"
};

try{
	arrival_val = $('means_of_arrival');

	if(arrival_val in arrival_val_mapped){
		arrival_mapped = arrival_val_mapped[arrival_val];
	}
}

 catch(e) {
	arrival_mapped = '';
}

if (arrival_mapped.length == 0) { arrival_mapped = 'other'; }

channelMap.put('means_of_arrival_mapped', arrival_mapped);

//create/update by and times
channelMap.put('qv_created_src', 'Mirth channel ' + channelName);
channelMap.put('qv_updated_src', 'Mirth channel ' + channelName);
var dateString = DateUtil.getCurrentDate('yyyyMMddHHmmss');
channelMap.put('qv_created_ts', dateString);
channelMap.put('qv_updated_ts', dateString);
