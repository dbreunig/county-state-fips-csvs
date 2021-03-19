# Handy County & State FIPS Codes

Many datasets with county-level information use [FIPS codes](https://en.wikipedia.org/wiki/FIPS_county_code) to identify the county. The canonical source for these codes is the American National Standards Institute. But we can find the current information at the US Census website [here](https://www.census.gov/geographies/reference-files/2019/demo/popest/2019-fips.html).

However, their files aren't prepared for easy ingestion. Further, many repositories on Github and beyond trip on a common 'gotcha' with FIPS codes: they're not numbers, they're strings, and they frequently have leading 0's. If you aren't paying attention, many csv libraries will helpfully interpret FIPS codes as integers and lop off that leading zero. The files found here are properly quoted to prevent that; however, if you use these files you still will have to manage this issue with whatever CSV library you're using.

Before you get mad at FIPS and curse its leading zeroes, please appreciate that FIPS codes are made up of multiple components. A FIPS county code, for example, is a 5 character code where the first two characters are the FIPS code for a state. So the code for Autauga County, Alabama is `01001`. The first `01` is the code for the state of the Alabama and the `001` code is the county identifier. You can express the `01` as `1` ([for example](https://wiki.openstreetmap.org/wiki/FIPS), Open Street Map expresses `state_fips` and `fips_code` in its metadata this way), but if you're combining them into composite strings (as we are here!) this will burn you later. Either because you'll be screwed by a missing zero or because you'll forget that county code segments are reused across states. [From the Census documentation](https://www.census.gov/programs-surveys/geography/guidance/geo-identifiers.html):

>FIPS codes for smaller geographic entities are usually unique within larger geographic entities. For example, FIPS state codes are unique within nation and FIPS county codes are unique within state. Since counties nest within states, a full county FIPS code identifies both the state and the nesting county. For example, there are 49 counties in the 50 states ending in the digits “001”. To make these county FIPS codes unique, the state FIPS codes are added to the front of each county (01001, 02001, 04001, etc), where the first two digits refer to the state the county is in and the last three digits refer specifically to the county.

So use the whole 5 character code for counties and don't drop the zero.

By the way, FIPS codes don't stop at counties. The next step down is Place codes. For example, Autaugaville is an unincorporated twon in Autauga County, Alabama. Its Place code is `03220`. So you could express it's full FIPS code, with prefixed state and county, as `0100103220`.

## Future Improvements:

- Provide easy to use state boundaries as .geojson and .csv using wkt strings.
- Provide easy to use county boundaries as .geojson and .csv using wkt strings.
- Provide Place FIPs codes and shapes.
