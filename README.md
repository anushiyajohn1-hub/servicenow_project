// OREY CODE - Institution Details Table Full Setup da!

// 1. Table create pannu (illa na thaan)
var table = new GlideRecord('sys_db_object');
table.addQuery('name', 'u_institution_details');
table.query();
if(!table.next()){
    table.initialize();
    table.name = 'u_institution_details';
    table.label = 'Institution Details';
    table.super_class = '';
    table.sys_name = 'Institution Details';
    table.insert();
    gs.print('Table Created: u_institution_details');
} else {
    gs.print('Table Already Exists da!');
}

// 2. Fields Add pannu da
function createField(name, label, type, refTable, maxLen, choiceList){
    var col = new GlideRecord('sys_dictionary');
    col.addQuery('name', 'u_institution_details');
    col.addQuery('element', name);
    col.query();
    if(col.next()){
        gs.print(label + ' Already Exists da');
        return;
    }
    col.initialize();
    col.name = 'u_institution_details';
    col.element = name;
    col.column_label = label;
    col.internal_type = type;
    if(refTable) col.reference = refTable;
    if(maxLen) col.max_length = maxLen;
    col.insert();
    gs.print(label + ' Created da!');

    // Choice add panna
    if(choiceList && choiceList.length > 0){
        for(var i=0; i<choiceList.length; i++){
            var ch = new GlideRecord('sys_choice');
            ch.initialize();
            ch.name = 'u_institution_details';
            ch.element = name;
            ch.label = choiceList[i];
            ch.value = choiceList[i];
            ch.sequence = i+1;
            ch.insert();
        }
    }
}

// Ippo 7 fields ah create pannu
createField('u_student_roll_number', 'Student Roll Number', 'string', null, 40, null);
createField('u_student_name', 'Student Name', 'reference', 'sys_user', 32, null);
createField('u_faculty_name', 'Faculty Name', 'reference', 'sys_user', 32, null);
createField('u_branch', 'Branch', 'choice', null, 40, ['ECE','EEE','CSE']);
createField('u_email', 'Email', 'string', null, 100, null);
createField('u_phone_number', 'Phone Number', 'string', null, 40, null);
createField('u_description', 'Description', 'string', null, 4000, null);

gs.print('FULL DONE DA! Institution Details Ready! 🔥');
