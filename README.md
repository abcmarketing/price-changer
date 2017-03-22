# price-changer
change price based on dropdowns

You can change the price in multiple divs with unique ids and multiple selections using the calculate_price function created in jquery.

The script is as follows:

<script type="text/javascript">  
    jQuery(document).ready(function($){

       $('body').on('click', '.strut-diameter', function(){
          var hardware =  document.getElementById( $(this).attr('data-hardware-field') );
          var qty =  document.getElementById( $(this).attr('data-qty-field') );

           if( $(this).val() == '0' ) {
               hardware.selectedIndex = 0;
               qty.selectedIndex = 0;
               hardware.setAttribute('disabled', true);
               qty.setAttribute('disabled', true);

               noOptionSelected($(this).data('group-id'));
           }
           else {
               hardware.removeAttribute('disabled');
               qty.removeAttribute('disabled');
               if(hardware.selectedIndex == 0) {
                   hardware.selectedIndex = 1;
               }
               if(qty.selectedIndex == 0) {
                   qty.selectedIndex = 1;
               }
               fillprice($(this).data('group-id'));
           }
       });
        $('.hardware, .qty').on('change', function(){
            fillprice($(this).data('group-id'));
        });
        $(document).on('click', '.other-hardware', function(){
            var hardware =  $(this)[0];
            var qty =  $('.other-qty')[0];

            if( $(this).val() == '0' ) {
                qty.selectedIndex = 0;
                qty.setAttribute('disabled', true);

                noOptionSelected($(this).data('group-id'));
            }
            else {

                qty.removeAttribute('disabled');
                if(qty.selectedIndex == 0) {
                    qty.selectedIndex = 1;
                }
                console.log('qty dropdown: '+ qty.selectedIndex)
                calculateOtherPrice($(this).data('group-id'));
            }

        });
        $('.other-qty').on('change', function(){
            calculateOtherPrice($(this).data('group-id'));
        });
    });
function calculate_price(groupid){
    var total = 0;
	var selector = '#fg_' + groupid + ' .calculate'; 
	
    $(selector).each(function() {
           var price = Number($(this).find('option:selected').data('price'));
        if(price){
            total += price;
        }
    });
    var qty = $('#qty_' + groupid).val();
    return (total*qty).toFixed(2);
}

$('#strut_diameter, #qty, #hardware').on('change',function(){
    $('#productprice').html('$' + calculate_price());
});

function fillprice(groupid){
	$('#productprice_' + groupid).html('$' + calculate_price(groupid));
}
function calculateOtherPrice(groupid) {
    var price = parseInt($('#other_price_'+groupid).val());
    var qty = parseInt($('#qty_' + groupid).val());
    var hardware = $('#hardware_' + groupid+'>option:selected');

    var total = (parseFloat(hardware.attr('data-price'))  * qty).toFixed(2);

    $('#productprice_' + groupid).html('$' + total);
}
function noOptionSelected(groupid) {
    $('#productprice_' + groupid).html('Select an option');

}
</script>
