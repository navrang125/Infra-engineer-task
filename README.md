# heycar Infrastructure Engineer take home test

## Air Quality API

You are on a venture team that is building a business in the environmental sector. As part of this effort, some engineers have built a small HTTP API in Golang that allows consumers to discover the air quality in their local area. The API receives latitude and longitude coordinates via a HTTP GET request, and subsequently retrieves air quality data for those coordinates by making a further HTTP request to the extneral [Air Visual API](https://airvisual.com/api) . Once the Air Visual response is received, our API returns a JSON response containing the nearest City to the requested coordindates along with the latest air quality index in that location. 

- Air quality URL: `/air-quality?lat=50&lon=1`

To aid with analytics and future evolution of the API, each incoming query is stored in a PostreSQL database, and a second external HTTP GET endpoint exposes all the recently queried cities.

- Queried cities URL: `/queried-cities`

As the venture continue and the business evolves, the API's responsibilities will inevitably change. It is also unknown what the future levels of traffic for the API could be, as this is dependent on the success of the venture.

# The Challenge - Part 1

The team have requested your assistance and expertise to help deploy the Air Quality API and associated database to Amazon Web Services. 

- Automate deployment of the Air Quality API to AWS
- You may use any tools or services you choose
- Make any code changes you feel are necessary to facilitate the deployment

Challenge tips:
- Think about how your solution will support future evolution and changes to the API
- We value simple solutions over anything complex. Don't overengineer
- This doesn't need to be production-ready code, but be prepared to explain what further changes you'd make if you had more time

AWS Access:
- The [AWS Free Tier](https://aws.amazon.com/free/) should provide you with enough free usage to complete this task. However, if you would like to use any services that fall outside the free tier, then we'll happily reimburse you for this usage (providing it's fair and you haven't just been mining Bitcoin)

# The Challenge - Part 2

The team have also requested your advice for their ongoing development and ownership of the Air Quality API.

Put together a short presentation that details:

- A proposed Continuous Delivery pipeline for the Air Quality API
- Changes that should be made to the API to facilitate its operation in production

Challenge tips:
- Consider security, resiliency, monitoring etc


## Initial Setup
- Install Docker locally from [https://docs.docker.com/install/](https://docs.docker.com/install/)
- Run `start.sh` to start the API
- Visit [http://localhost:5000](http://localhost:5000) in a browser to confirm the API is running
- Run `test.sh` to execute tests