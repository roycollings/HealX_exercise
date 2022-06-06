# Setup
Before starting, download the metadata file to the appropriate place (approx. 70M):
```
curl https://ai2-semanticscholar-cord-19.s3-us-west-2.amazonaws.com/2020-04-03/metadata.csv --output back-end/model/metadata.csv
```

# Running
I didn't get time to dockerise this, so it's a little manual for now. You need 2 terminals:

_terminal 1:_
```
cd back-end
python3 -m venv .env3.10
source ./.env3.10/bin/activate
python3 -m pip install --upgrade pip
pip install -r requirements.txt
uvicorn main:app --reload --host 0.0.0.0
```

_terminal 2:_
```
cd front-end
yarn
JOURNALS_FILE="model/metadata.csv" yarn start
```

Now the api should be running as a service on http://localhost:8000 (with the usual full swagger docs on `/docs` and `/redoc`), and the web page will be on http://localhost:3000/.

# Disclosure
I didn't this in one sitting (3 hours outside of office hours is hard to find), but it _did_ take longer than 3 hours (probably more like 4 - I was caught off guard by enzyme support seemingly being absent for React 18).
The end result is 'functional' but unfinished, but is on the intended track.
# Initial plan
This may change as I go, but I wanetd to log changes in different README commits to record my journey.

## Principles
- Conform to standard approaches / patterns as a 'first choice' in all decisions.
- A 'test-first' approach where possible. _Always verify tests will fail when they should._
- Consider what wouold be important to the user when prioritising.

## Phase 1 (MVP)
- Deliver search results from a REST api.
- Search and display results in UI
- Simple UI
- Github workflows for tests (on both projects)
- Dockerise

## Phase 2
- Paginate results
- Bookmark journals

## Phase 3
- Improve ui / more focus on css etc...

# Decisions

## FastAPI back end
I wrote a Typescript api not too long ago (still have access to the repo for it), so would've been fairly trivial to adapt that. However, you use Python/FastApi, I've used Python before (not for an api) and wanted to get familar with FastApi. It seems very quick to write, so I figure the adventure is worth the risk! :)

### REST
I chose REST instead of GraphQL. I was _tempted_ to try GraphQL too, but felt given the time I had it would've been too much to get 'right enough' in the timescale.

## Typescript + react front end
I've used React before and I'm pretty comfortable with Typescript. Again, this is what's used in HealX, so I'd rather cconform to that.

### No store
Only used 'Mobx' in the past, so I did a couple of Redux tutorials (very cool!), but ultimately don't think I need it to acheieve the goal. Following the philosophy to avoiod unrequired dependencies and complexity I decided to avoid it and just use local state.

This is also in-ilne with the recommendation on 'when do you need redux?' (https://twitter.com/dan_abramov/status/699241546248536064) ...
> "... don’t use Redux until you have problems with vanilla React"
[Dan Abramov (one of Redux's creators)]


## Monorepo
To keep things simple for demo'ing etc... I'm putting back end and front end in the same repo.
In 'real life' I'd usually adivse against that, but for this exercise it makes sense.
