## How to add a new link in your doctype dashboard ?

First you have to write a function in custom script which is called everytime when
you open the doctype.
for example I have to add a link of Commitment Doctype in my customer doctype.

```
frappe.ui.form.on('Customer', {
	refresh: function(frm) {
		dashboard_link(frm, "Commitment Doctype");
	}
});

```

Now dashboard_link function consist of jquery which is responsible to :-

1. Add the link in the given position
1. Add template which is similar to the other link.
1. Show no. of associated doctype.
1. Add button to make a new one.

This is the template needed.

```
frappe.templates["dashboard_link"] = ' \
    	<div class="document-link" data-doctype="{{ doctype }}"> \
    	<a class="badge-link small">{{ __(doctype) }}</a> \
    	<span class="text-muted small count"></span> \
    	<span class="open-notification hidden" title="{{ __("Open {0}", [__(doctype)])}}"></span> \
    		<button class="btn btn-new btn-default btn-xs hidden" data-doctype="{{ doctype }}"> \
    				<i class="octicon octicon-plus" style="font-size: 12px;"></i> \
    		</button>\
    	</div>';

```

This is the function needed to add no. of associated doctype. In this function
GET method is used to call the api which will return the no. of commitment doctype
associated with the customer and place next to the link field.

```
var set_open_count = function (frm, doctype){
    frappe.call({
        type: "GET",
        method: "vhms.api.get_open_count",
        args: {
            customer: frm.doc.customer_name,
        },
        callback: function(r) {
            $.each(r.message.count, function(i, d) {
                frm.dashboard.set_badge_count("Commitment Doctype", cint(d.open_count), cint(d.no));
            });
        }
    });
};

```

This is the parent function and the function and template defined above are used
in this function.
Flow of the function:-

1. Find the element to place the new field
1. Append the new element with the template defined ie. dashboard_link
1. Place the no. of associated doctype by calling set_open_count function.
1. Add link to the field with filters when clicked on.
1. Placed a button to make new doctype.

```
var dashboard_link = function (frm, doctype){
    var parent = $('.form-dashboard-wrapper [data-doctype="Payment Entry"]').closest('div').parent();

    parent.find('[data-doctype="'+doctype+'"]').remove();
    parent.append(frappe.render_template("dashboard_link", {"doctype":doctype}));


    var self = parent.find('[data-doctype="'+doctype+'"]');

    set_open_count(frm, doctype);

    // bind links
    self.find(".badge-link").on('click', function() {
        frappe.route_options = {"customer": frm.doc.name};
        frappe.set_route("List", doctype);
    });

    // bind new
    if(frappe.model.can_create(doctype)) {
        self.find('.btn-new').removeClass('hidden');
    }
    self.find('.btn-new').on('click', function() {
        frappe.new_doc(doctype,{
            "customer": frm.doc.name
        });
    });
};

```

Credit: Rishikesh Samantra via https://gist.github.com/rsqwerty/49809a40e7edfd09ce4776159f9c2891
