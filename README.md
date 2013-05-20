==============================
django-zipdistance
==============================

django-zipdistance is a simple application to find the distance
between any two zip codes.  The fixture provided with this application
is derived from the Year 2000 Zip Code Tabulation Area (ZCTA) tables
provided by the US Census Bureau.  The Census Bureau notes:

    ZCTAs are generalized area representations of U.S. Postal Service
    (USPS) ZIP Code service areas.  In most instances the ZCTA code
    equals the ZIP Code for an area.  Some ZIP Codes represent very
    few addresses (sometimes only one) and therefore will not appear
    in the ZCTA database.  ZCTA is a trademark of the U.S. Census
    Bureau; ZIP Code is a registered trademark of the U.S. Postal
    Service.  

In short, the ZCTAs most likely, but are not guaranteed to, correspond
with ZIP Codes for any given address.  In testing, this database
proved to be entirely acceptable for most of the United States.  But
just to make sure, I repeat: THE SOFTWARE AND DATA FILES ARE PROVIDED
"AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING
BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A
PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS
OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE,
ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
OTHER DEALINGS IN THE SOFTWARE.

-----------------------------

Setup: 

1. Add to ```zipdistance``` to ```INSTALLED_APPS`` at ```settings.py```
2. Import zipcodes from initial_data_manual.json (TODO: command-line step)


Template tag usage example: 

    {% load zipdistance %}
    Distance is: {% distance user.profile.zipcode to mall.zipcode %} miles

Standard Usage:

Find all the zipcodes within 50 miles:

    from zipdistance.models import ZipDistance
    target = ZipDistance.objects.get(zipcode = zip_form.cleaned_data['zipcode1'].strip())
    zips = ZipDistance.objects.distance_from(target, 50)

Find the distance between two zipcodes:

    from zipdistance.models import ZipDistance
    zip1 = ZipDistance.objects.get(zipcode = zip_form.cleaned_data['zipcode1'].strip())
    zip2 = ZipDistance.objects.get(zipcode = zip_form.cleaned_data['zipcode1'].strip())
    zip1.distance_between(zip_2)

Find all the stores within a given distance.  

    from zipdistance.models import ZipDistance
    from yourapp.models import Store
    zips = ZipDistance.objects.distance_from(target, 50)
    stores = Store.objects.get(zipcode__in = [z.zipcode for z in zips])

That last one's not terribly efficient.  If you used the ZipMap as a
way of storing zipcodes, you could probably come up with something
smarter.

